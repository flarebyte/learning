# Theme data

`ThemeData` in Flutter is a configuration class that centralizes visual styling and layout properties across an app, enabling consistent theming. It defines attributes such as colors, typography, shapes, padding, and component styles for various widgets, like buttons, text fields, and dialogs.

By using `ThemeData`, you can establish both light and dark themes, set primary and accent colors, customize text styles, and apply global widget styling. `ThemeData` can be combined with `Theme.of(context)` to access and apply theme properties throughout the widget tree, ensuring uniform styling and simplifying theme switching and maintenance.

## Bespoke theme

To create a bespoke theme with `ThemeData`, you should define:

1. **Color Scheme**: Customize `primaryColor`, `accentColor`, `backgroundColor`, and `errorColor` for consistent coloring.
2. **Typography**: Set custom `TextTheme` for headings, body, captions, etc., to standardize font styles.
3. **Component Themes**: Specify styles for components like `AppBarTheme`, `ButtonTheme`, `InputDecorationTheme`, and `CardTheme`.
4. **Shape and Elevation**: Customize `shape` properties (e.g., rounded corners) and elevation for widgets like buttons and dialogs.
5. **Padding and Margin**: Adjust `visualDensity` and spacing attributes for refined layout adjustments.

This configuration ensures a unique, consistent look and feel across the app.

## BoxDecoration

`BoxDecoration` in Flutter is a decoration class primarily used to style `Container` widgets by providing background color, gradient, border, shape, shadows, and images. It defines how the container’s visual appearance should look.

### Best Way to Theme `BoxDecoration`

To theme `BoxDecoration`, avoid hard-coding values directly in each `Container`. Instead, define reusable `BoxDecoration` styles in your theme by creating `BoxDecoration` properties as constants or static methods within a custom theme class. For instance:

1. **Color and Gradients**: Use `Theme.of(context).colorScheme` for colors, ensuring that decoration colors adjust with light/dark themes.
2. **Shape and Border**: Store shapes and border styles in a central class or as part of the theme to maintain consistency.
3. **Shadows**: Define reusable shadows using constants or theme extensions to avoid redundancy.

For theming consistency, access these custom decorations through your theme context or via helper methods, ensuring they are easy to update and maintain across the app.

## Theme classes

Here's a table summarizing the standard Flutter theme classes:

| Theme Class                       | Example in `ThemeData`                                       | Main Fields                                                          | Description                                                                                                            |
| --------------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **TextTheme**                     | `textTheme`                                                  | `bodyLarge`, `displayLarge`, `titleMedium`, ...                      | Defines text styles for various text elements throughout the app, such as headings and body text.                      |
| **ColorScheme**                   | `colorScheme`                                                | `primary`, `secondary`, `background`, `error`, ...                   | Specifies a color palette used by other theme elements, providing primary, secondary, and other key colors.            |
| **AppBarTheme**                   | `appBarTheme`                                                | `backgroundColor`, `elevation`, `titleTextStyle`, ...                | Customizes the appearance of `AppBar` widgets, allowing control over color, shadow, and text styles.                   |
| **ButtonStyle**                   | `buttonTheme`, `elevatedButtonTheme`, `textButtonTheme`, ... | `backgroundColor`, `elevation`, `padding`, ...                       | Configures button styles (e.g., raised, flat) for consistent appearance and behavior across button types.              |
| **IconThemeData**                 | `iconTheme`, `primaryIconTheme`                              | `color`, `size`, `opacity`                                           | Defines color, size, and opacity for icons, allowing consistent icon styling in different contexts.                    |
| **InputDecorationTheme**          | `inputDecorationTheme`                                       | `fillColor`, `border`, `labelStyle`, `hintStyle`, ...                | Styles input fields like `TextField`, allowing customization of colors, borders, and text styles for labels and hints. |
| **CardTheme**                     | `cardTheme`                                                  | `color`, `elevation`, `margin`, `shape`                              | Controls the look of `Card` widgets, including background color, border radius, and shadow.                            |
| **SliderThemeData**               | `sliderTheme`                                                | `thumbColor`, `activeTrackColor`, `inactiveTrackColor`, ...          | Styles sliders for consistent colors and shapes in track, thumb, and active regions.                                   |
| **DialogTheme**                   | `dialogTheme`                                                | `backgroundColor`, `elevation`, `titleTextStyle`, `contentTextStyle` | Styles dialogs, allowing customization of background color, elevation, and title/content text styles.                  |
| **BottomNavigationBarThemeData**  | `bottomNavigationBarTheme`                                   | `backgroundColor`, `selectedItemColor`, `unselectedItemColor`, ...   | Customizes the bottom navigation bar appearance, setting colors for items and background.                              |
| **BottomAppBarTheme**             | `bottomAppBarTheme`                                          | `color`, `elevation`, `shape`                                        | Defines the style of the `BottomAppBar`, including color, shadow, and shape.                                           |
| **TabBarTheme**                   | `tabBarTheme`                                                | `labelColor`, `unselectedLabelColor`, `indicator`                    | Customizes `TabBar` colors, styles, and indicator appearance for consistent tab styling.                               |
| **ChipThemeData**                 | `chipTheme`                                                  | `backgroundColor`, `labelStyle`, `shape`, `padding`                  | Styles `Chip` widgets, allowing customization of colors, text styles, and shape.                                       |
| **CheckboxThemeData**             | `checkboxTheme`                                              | `fillColor`, `checkColor`, `shape`, `side`                           | Configures checkboxes' appearance, including color, shape, and border.                                                 |
| **SwitchThemeData**               | `switchTheme`                                                | `thumbColor`, `trackColor`, `materialTapTargetSize`                  | Styles switches, defining colors for thumb and track, and optional target sizing.                                      |
| **RadioThemeData**                | `radioTheme`                                                 | `fillColor`, `overlayColor`, `shape`                                 | Defines appearance settings for radio buttons, including fill and overlay colors, and shape.                           |
| **PopupMenuThemeData**            | `popupMenuTheme`                                             | `color`, `elevation`, `textStyle`, `shape`                           | Customizes pop-up menus for color, shadow, text style, and shape.                                                      |
| **FloatingActionButtonThemeData** | `floatingActionButtonTheme`                                  | `backgroundColor`, `foregroundColor`, `elevation`, ...               | Configures Floating Action Buttons with colors, icon styling, and elevation options.                                   |
| **TooltipThemeData**              | `tooltipTheme`                                               | `textStyle`, `verticalOffset`, `decoration`, `padding`               | Styles tooltips, allowing customization of text, positioning, and background decoration.                               |
| **ProgressIndicatorThemeData**    | `progressIndicatorTheme`                                     | `color`, `circularTrackColor`                                        | Sets colors and track styling for progress indicators (linear and circular).                                           |
| **DividerThemeData**              | `dividerTheme`                                               | `color`, `thickness`, `indent`, `endIndent`                          | Configures divider lines for color, thickness, and spacing, used for sectioning content.                               |
| **ListTileTheme**                 | `listTileTheme`                                              | `tileColor`, `selectedColor`, `textColor`, `iconColor`               | Sets colors and styles for list tiles, impacting color and text/icon styling.                                          |

This table provides an overview of each theme class, its main properties, and the purpose it serves within `ThemeData`.
