# Drawing a rectangle

Now that we have somewhere to draw things, why don't we start drawing something in there?

In computer graphics almost everything is drawn with triangles. However, in 2D games most of the times we use rectangles, so why don't we start by drawing one on our screen? Since it is so common, our FRUG instance includes a function that draws a rectangle given a position, the width, the height, and the color we want for our rectangle.

Now, even though it is easy to draw the rectangle, it is also important to clear things up. FRUG works by storing a list of all the shapes it should draw each frame, so it is important to avoid drawing the same shapes we used on previous frames (or maybe for some reason you want to do that), to do that we should add in our update function a call to our instance's `clear` function, that way we forget about the previous frames' shapes and we can start anew.

```rust
instance.clear();
```

Now we can start drawing a rectangle with the following line (after the one we just added):

```rust
instance.clear();
instance.add_colored_rect(0.0, 0.0, 0.75, 0.5, [0.0, 0.5, 0.5]); // <- NEW!
```

Those first 4 numbers we're passing as parameters might look logical to you, but the other three, in case you missed it, defines the red, green, and blue parts of the color we'll be using in our rectangle.

If you run this you will notice that nothing shows up on the screen and that the weird list of wgpu errors are still happening. This is because FRUG is storing all that is needed to draw that rectangle in a staging buffer, meaning that we're actually never saying to FRUG to draw that rectangle. For that we need to add the following line to ask our instance to send the data in that staging buffer to the actual buffer which will be drawn onto the screen:

```rust
instance.clear();
instance.add_colored_rect(0.0, 0.0, 0.75, 0.5, [0.0, 0.5, 0.5]);
instance.update_buffers(); // <- NEW!
```

Now, if we run the project we should be able to see our rectangle nicely showing up in our screen.

![The window and our rectangle](../img/guide_rectangle.png)

In the end, your `main.rs` file should look like this:

```rust
use frug;

fn main() {
    let (frug_instance, event_loop) = frug::new("My Window");

    let update_function = move |instance: &mut frug::FrugInstance, _input: &frug::InputHelper| {
        instance.clear();
        instance.add_colored_rect(0.0, 0.0, 0.75, 0.5, [0.0, 0.5, 0.5]);
        instance.update_buffers();
    };

    frug_instance.run(event_loop, update_function);
}
```

Next up... changing the background color!