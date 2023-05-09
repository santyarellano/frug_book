# Pong

Pong is sometimes considered as the "Hello World" of games. A simple game which can let's us explore how things work. With FRUG this won't be the exception and we'll develop a simple version of Pong to explore how to use FRUG beyond simple feature examples. 

The first thing we'll need is our `main.rs` with our frug instance initialization, our empty update function, and the `run` call. You should have something like this:

```rust
use frug;
use cgmath::Vector2;

fn main() {
    let (frug_instance, event_loop) = frug::new("Pong!");

    let update_function = move |instance: &mut frug::FrugInstance, input: &frug::InputHelper| {
        // Rendering
        instance.clear();
        instance.update_buffers();
    };

    frug_instance.run(event_loop, update_function);
}
```

Oh, and don't forget to add FRUG to your `Cargo.toml`! (we'll also need another crate called `cgmath` so you might add it as well).

```toml
# ...

[dependencies]
cgmath = "0.18"
frug = "*"
```


Now, let's quickly go over what things we'll need, shall we?

* Our "player" (just a rectangle to represent the player's paddle/racket).
* The oponent (almost the same as the player).
* The ball

We could technically need more things, but for the sake of simplicity we'll only use those three things. Now, again for simplicity we'll just use rectangles for all 3 objects, yet the ball will obviously be smaller (a small square). This "ball" will start at the center of the screen and move to a random side of the screen to start the game. The "opponent" will just move to try to align the vertical center of the paddle with the center of the ball. And finally, we'll just let the player control their paddle through the keyboard's arrows.

Let's start by creating the ball and the opponent, as it allows us to start developing the simple aspects of our game right away.

We can start by creating a struct called "CollisionRectangle" which will help us to manage our 3 objects:

```rust
struct CollisionRectangle {
    pos: Vector2<f32>,
    width: f32,
    height: f32,
    vel: Vector2<f32>,
}
```

> You might notice that we're using `Vector2<i32>` for our position and velocity. This might look familiar if you've used Unity or other engines, but in case you don't know them just keep in mind that they will simply hold 2 components (x and y) for us to represent those attributes.

And we could also use a method to initialize new instances of our struct:

```rust
impl CollisionRectangle {
    fn new(x: f32, y: f32, width: f32, height: f32) -> CollisionRectangle {
        CollisionRectangle {
            pos: Vector2 { x, y },
            width,
            height,
            vel: Vector2 { x: 0.0, y: 0.0 },
        }
    }
}
```

Now that we have our struct, we can define our ball! (do this inside your main function but before your update function)

```rust
// ...

// our objects
let mut ball: CollisionRectangle = CollisionRectangle::new(-0.05, -0.05, 0.1, 0.1);
```

And we can we could add such object to an array which we'll use to render our objects.

```rust
// our renderable array
let to_render = vec![&ball];
```

Now all we need to do render such objects! (this goes in our update function after the `clear()` function call)

```rust

```