# Display only

* Draw without including the runtime
* No widgets
* No templates

## Example

An example showing the use of just the `display` crate:

```rust
use anathema::display::{size, Color, Screen, ScreenPos, Style};
use std::io::stdout;

fn main() {
    let mut output = stdout();
    let size = size().unwrap();
    let mut screen = Screen::new(&mut output, size).unwrap();

    let _ = screen.enable_raw_mode();

    // Enter an alternative screen, preserving the output
    // before the program runs.
    let _ = screen.enter_alt_screen(&mut output);

    // Clear the entire screen.
    // let _ = screen.clear_all(&mut output);

    let mut style = Style::new();
    style.set_fg(Color::Yellow);
    style.set_bg(Color::Red);

    for x in 2..12 {
        for y in 3..8 {
            screen.put('x', style, ScreenPos::new(x, y));
        }
    }

    let _ = screen.render(&mut output);

    std::thread::sleep(std::time::Duration::from_secs(1));

    // Restore the terminal
    let _ = screen.restore(&mut output);
}

```
