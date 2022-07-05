# Partial pop-out of a DevExtreme component.

## Problem

We are trying to move Dom elements of a DevExtreme component to another window. Like a partial Pop-out functionality.

We were able to move Dom elements, but dialogs (like column chooser) were appearing on the previous window. So we intercepted appendChild of body of the existing window, in order to move appended nodes to the new windows (window in focus) body. And was able to show dialogs in the new window.

But we couldn't find a way to make the event system stuff work. Like dragging or resizing of columns, or dragging of dialogs (like column chooser). Even though we intercepted addEventListeners of body and document of the previous window, to move them to the new window, we were unsuccessfull to make event system stuff work on the new window. This part is not included in this project, because it does not work anyway.

## Case reproduction

1. Do the project setup and execute.
2. Browse to http://localhost:8080/
3. Observe executing cases on the initial window.
    * Press Column chooser and drag the opening dialog. Observe successfull drag.
    * Drag columns and change ordering. Observe successfull drag.
    * Resize column by dragging. Observe successfull resize.
4. Press the popUp button, down below the table. A new window pops out and the table is moved to the new popped out window.
5. Observe failing cases on the new window.
    * Press Column chooser and drag the opening dialog. Observe unsuccessfull drag.
    * Drag columns and change ordering. Observe unsuccessfull drag.
    * Resize column by dragging. Observe unsuccessfull resize.
6. Also scrolls inside the dialogs are working only on mac and not windows. (Edge 103 and Chrome 103 on mac vs windows)


## Project setup
```
npm install
```

## Project execute
```
npm run serve
```