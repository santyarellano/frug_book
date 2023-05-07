# Setting fullscreen and window size

Some times we don't want to have our games in a small window, maybe we want to have our game using the entirety of our screen. For this the FRUG instance has a function called `set_fullscreen` which receives a bool to indicate whether we want to set our window to fullscreen or not.

Therefore it would look like this:

```rust
// This sets our window to fullscreen
frug_instance.set_fullscreen(true);

// This makes us exit the fullscreen mode.
frug_instance.set_fullscreen(false);
```

Now, what if we don't want to use the whole screen but we want our window to have a specific size? For this our instance has a method called `set_window_size` which we can use to set both the width and height of our window, all we need to do is pass as paramters both numbers and consider it done!

```rust
frug_instance.set_window_size(550.0, 500.0);
```