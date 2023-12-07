# Base

Initializing a godot rust file is at times a lot of boilerplate. In the best case you have something like this:

```rust
use godot::prelude::*;
use godot::engine::{Node};

#[derive(GodotClass)]
#[class(init, base=Node)]
pub struct YourThing {
  #[base]
  base: Base<Node>,
}
```

This will get you pretty far. However the moment you need to have something in your init block you creep up in line count.

```rust
use godot::prelude::*;
use godot::engine::{Node, INode};

#[derive(GodotClass)]
#[class(base=Node)]
pub struct YourThing {
  #[base]
  base: Base<Node>,
  some_other_thing: String,
}

#[godot_api]
impl INode for YourThing {
  fn init(base: Base<Node>) -> Self {
    YourThing {
      base,
      some_other_thing: "Something".to_string()
    }
  }
}
```

Its times like these where I like to have a snippet tool or macro to speed things up. Allowing you to boilerplate out fast and keep moving on the project.

But you can make that init just a little bit better. Here is the pattern I like.

```rust
fn init(base: Base<Self::Base>) -> Self {
  YourThing {
    base,
  }
}

```

Its not much but I like it. Functionally the same, just one less thing to think about. 
