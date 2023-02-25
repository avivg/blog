---
title: Rust and Opaque C Structures
date: 2023-02-25 18:00 +02:00
tags: [Rust, C, FFI, C-API]
---
## C-Style Opaque Structure and Rust FFI

I finally got around to trying Rust in my day job. The first thing I needed is to make sure I know how anything I create can interract with our product (NN compiler). The API I wanted it to interract with, is a C-style API, which can be redeclared in rust pretty easily using the built-in standard FFI.

The challenge was with opaque structures that are forward-declared in the API and use double pointer handles for creation. This is a pretty common pattern in C-Style APIs, so I was surprised when I needed to look in several different places and apply some guess work on how to use it in Rust.

All the examples in this post are available in [github](https://github.com/avivg/rust-opaque-example).

Suppose we have a C-API similar to:

```C
// mylib.h
typedef enum {
    SUCCESS,
    FAIL
} status_t;

struct opaque_t;
typedef struct opaque_t* handle_t;

status_t mylib_create(handle_t* handle);
status_t mylib_destroy(handle_t handle);
status_t mylib_set(handle_t handle, int value);
status_t mylib_dump(handle_t handle);
```

The implementation doesn't really matters, but in the [full example](https://github.com/avivg/rust-opaque-example), it does some malloc, free, managing and integer field and dumps stuff.

The first difficulty is how to define this opaque ```handle_t``` pointer. I found the answer in this [stack-overflow answer](https://stackoverflow.com/a/38315613/4016231), and it worked like a charm. I ended up with:

```Rust
#[repr(C)]
pub struct CMyLib {
    _f: [u8; 0],
}
pub type MyLibHandle = *mut CMyLib;
```

Which I wanted to create with style, so I added:
```Rust
impl CMyLib {
    pub fn handle() -> MyLibHandle {
        std::ptr::null_mut()
    }
}
```

So now that I had my opaque struct pointer, add the declarations:

```Rust
#[derive(Debug)]
#[repr(C)]
pub enum Status {
    Success,
    Failure,
}

#[link(name = "mylib")]
extern "C" {
    fn mylib_create(handle_ptr: *mut MyLibHandle) -> Status;
    fn mylib_destroy(handle: MyLibHandle) -> Status;
    fn mylib_set(handle: MyLibHandle, value: i32) -> Status;
    fn mylib_dump(handle: MyLibHandle) -> Status;
}
```

Followed by a Rust-style struct to wrap this object in "safe" code:
```Rust
pub struct MyLib {
    handle: MyLibHandle,
}
```

When it came to the double pointer in ```mylib_create```, I didn't find any text book solution. I used some guess-work and deduction from various posts and ended up with:
```Rust
impl MyLib {
    pub fn create() -> Result<Self, String> {
        let mut handle = CMyLib::handle();
        match unsafe { mylib_create(std::ptr::addr_of_mut!(handle)) } {
            Status::Success => Ok(Self { handle }),
            err_code => Err(format!("Failed to create MyLib. Error: {err_code:?}")),
        }
    }
    ...
```

