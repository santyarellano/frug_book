# Camera and user input

If you are familiar with how WGPU works you might know this, but in case you don't, we have been working with 3D! I've tried to simplify some functions to work with 2D but when it comes to the camera I thought it was better to leave some control for developers so they can work with 3D if they want. If you want to go in depth with how the camera works I suggest going through those chapters in [Learn WGPU](https://sotrh.github.io/learn-wgpu/beginner/tutorial6-uniforms/). However, in here I'll just leave it at telling you that our FRUG instance has a property called `camera` which is a struct. Such struct contains some properties but the ones you can use to get started are the `eye` (a 3D point that defines where the camera is located) and the `target` (another 3D point which defines where our camera is looking at).

In this section we'll take a look at how we can move our camera using the arrow keys, for this we'll use the second property that appears in the definition of our update function called `input`. 

> `input` is actually [WGPU's Input Helper](https://docs.rs/winit_input_helper/latest/winit_input_helper/struct.WinitInputHelper.html). Check it out if you wish learn more about such functions.

For this let's start by checking if our right key is being held inside our update function and before all the rendering-related lines.

```rust
let update_function = move |instance: &mut frug::FrugInstance, input: &frug::InputHelper| {
    // Checking our input
    if input.key_held(frug::VirtualKeyCode::Right) { // 1.
        // 2. We'll write something to move our camera in here
    }

    // Rendering
    instance.clear();
    instance.add_tex_rect(-0.25, 0.0, 0.5, 0.5, frog_text_idx);
    instance.update_buffers();
};
```

1. You surely noticed that we wrote something saying `VirtualKeyCode`. FRUG has some components that help use work with the `input` struct, in this case we use that enum to check for the right key. Later on we'll do the same for the left key.

2. In there we'll write the code to move our camera 

Now, like I mentioned before, our camera struct has the `eye` and `target` properties, which we'll use to move our camera. Since we want to keep this as 2D as possible we will move both the `eye` and `target` properties of the camera in the same part and at the same rate. In this case we'll want to move things horizontally, so we'll use the `x` property of both of them with the following lines:

```rust
instance.camera.eye.x -= 0.01;
instance.camera.target.x -= 0.01;
```

If you run your project you'll notice that when you press the right key our image nicely moves to the right side of the screen. This means that the camera is moving, not the frog itself, yet it creates the illusion that the frog is moving.

Now lets do the same thing for the left, shall we?

```rust
else if input.key_held(frug::VirtualKeyCode::Left) {
    instance.camera.eye.x += 0.01;
    instance.camera.target.x += 0.01;
}
```

If you run this you'll notice that we now can move things to either side (horizontally), yet you'll probably notice that if you press both of them it moves towards the right side of the screen. This is because we did things in a poor manner. You could implement your own version of this to avoid this, or if you want you can check it out in the platformer example, where things will be a bit better, yet we'll leave it like that for the sake of simplicity in this example.

As always, here's the whole version of the code you should have up until now (don't forget that you should use your own image for the texture!).

```rust
use frug;

fn main() {
    let (mut frug_instance, event_loop) = frug::new("My Window");

    let img_bytes = include_bytes!("frog.png");
    let frog_text_idx = frug_instance.load_texture(img_bytes);

    let update_function = move |instance: &mut frug::FrugInstance, input: &frug::InputHelper| {
        // Checking our input
        if input.key_held(frug::VirtualKeyCode::Right) {
            instance.camera.eye.x -= 0.01;
            instance.camera.target.x -= 0.01;
        } else if input.key_held(frug::VirtualKeyCode::Left) {
            instance.camera.eye.x += 0.01;
            instance.camera.target.x += 0.01;
        }

        // Rendering
        instance.clear();
        instance.add_tex_rect(-0.25, 0.0, 0.5, 0.5, frog_text_idx);
        instance.update_buffers();
    };

    frug_instance.run(event_loop, update_function);
}
```

Now let's check out how to draw custom shapes, shall we?