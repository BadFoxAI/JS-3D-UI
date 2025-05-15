# JS-3D-UI

A lightweight JavaScript library for creating interactive 3D user interfaces within a Three.js scene. Supports desktop mouse and is structured for WebXR interaction.

Try Here at: [https://js-3d-ui.netlify.app/](https://js-3d-ui.netlify.app/)

## Features

*   **Core UI Elements:** Text Labels, Buttons, Text Inputs.
*   **Declarative Menus:** Define UI structures with JavaScript objects.
*   **Theming & Styling:** Customizable appearance via global defaults, themes, and per-element styles.
*   **Interaction:** Mouse-based hover, click, and focus. Keyboard input for text fields. Basic WebXR integration.
*   **Dynamic Textures:** UI elements rendered on 2D canvas textures.
*   **Extensible:** Base classes for creating new UI components and interaction methods.

## Quick Usage

1.  **Include Three.js** in your HTML project (e.g., via import map).
2.  **Copy the JS-3D-UI `<script type="module">...</script>`** content into your HTML file or a separate JS module.
3.  **Define a Menu:**
    ```javascript
    const myMenuDefinition = {
        id: 'mainMenu',
        position: new THREE.Vector3(0, 1.5, -2), // In 3D space
        items: [
            { name: "infoText", type: 'textlabel', text: "Welcome!" },
            { name: "actionBtn", type: 'button', text: "Click Me", actionKey: "myAction" }
        ]
    };
    ```
4.  **Initialize the UI System:**
    ```javascript
    // (After Three.js scene, camera, renderer setup)
    const uiRenderer = new ThreeJSRenderer(scene, camera);
    const menuManager = new MenuManager(uiRenderer);
    const mouseHandler = new MouseInteractionHandler(renderer.domElement, camera);
    menuManager.setInteractionHandler(mouseHandler);

    menuManager.registerMenuDefinition(myMenuDefinition);
    menuManager.registerAction("myAction", () => console.log("Button Clicked!"));
    menuManager.loadMenu('mainMenu');
    ```
5.  **Update in Animation Loop:**
    ```javascript
    function animate() {
        // ... your other updates ...
        if (menuManager) menuManager.update();
        renderer.render(scene, camera);
    }
    ```

## Core Concepts

*   **`UIElement3D`:** Base class for all visual components (buttons, labels, inputs). Handles style, state, and basic interactions.
*   **`MenuManager`:** Central controller. Manages menu definitions, themes, actions, and the active `Menu`.
*   **`Menu`:** A collection of `UIElement3D` instances, built from a definition. Handles its items' layout and visibility.
*   **`ThreeJSRenderer` (UI-specific):** Bridges the UI system with Three.js for creating menu groups and applying layouts. Not the main WebGLRenderer.
*   **Interaction Handlers (`MouseInteractionHandler`, `XRControllerInteractionHandler`):** Manage input from different sources and translate it to UI events.
*   **Menu Definitions:** JavaScript objects describing a menu's ID, position, and an array of `items`. Each item has a `type`, `name`, `text`/`value`, optional `style`, and `actionKey` (for buttons).

## Customization

*   **Styling:** Modify `GLOBAL_DEFAULT_STYLE`, define new `THEMES`, or provide `style` objects in menu/item definitions.
*   **New Elements:** Extend `UIElement3D`, implement `createMesh()`, and update `Menu._buildItems()`.
*   **Actions:** Use `menuManager.registerAction("actionKey", callback)` and link via `actionKey` in button definitions.

## Future Enhancements

*   Full WebXR controller support.
*   Scrollable containers.
*   Additional UI elements (sliders, checkboxes).
*   Advanced layout options.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

## License

MIT License. (See LICENSE.txt if provided, otherwise assume standard MIT terms).
