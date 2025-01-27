# Scrolling

Here’s an overview of the responsibilities and interactions for `Viewport`, `Scrollable`, `ScrollView`, and `Scrollbar`:

- **Viewport**

  - Clipping area showing only visible content.
  - Manages transformations based on scroll position.
  - Optimizes performance by rendering only on-screen content.
  - Works closely with `Scrollable` to display the correct section of content.

- **Scrollable**

  - Core scroll behavior, handling user gestures and scroll physics.
  - Manages scroll position and updates it based on user interactions.
  - Coordinates with `ScrollController` for programmatic scroll control.
  - Provides the foundational scrolling mechanics for `ScrollView`.

- **ScrollView**

  - High-level, abstract class for scrollable widgets like `ListView` and `GridView`.
  - Configures scrolling direction, physics, and layout structure.
  - Instantiates and manages `Scrollable` and `Viewport` to handle scrolling.
  - Facilitates customizable scrolling behaviors for various scrollable UIs.

- **Scrollbar**
  - Visual indicator for the current scroll position and content extent.
  - Hooks into `Scrollable` to update its position based on scroll events.
  - Can be configured to appear dynamically or persistently based on scrolling.
  - Enhances user experience by providing context on scrolling progress.

**Interactions**:

- `ScrollView` provides the high-level interface, internally using `Scrollable` and `Viewport` to manage content display.
- `Scrollable` handles gestures and updates the `Viewport` to display the appropriate content.
- `Viewport` renders only the visible portion, ensuring performance.
- `Scrollbar` reads position from `Scrollable`, offering a visual guide for scroll status.
