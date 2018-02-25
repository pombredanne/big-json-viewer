# Big JSON Viewer


[![npm](https://img.shields.io/npm/v/big-json-viewer.svg)](https://www.npmjs.com/package/big-json-viewer)


A JavaScript library that enables efficient working with large JSON data in the browser.

The JSON data is held as ArrayBuffer and never parsed completely.

Information about the top level nodes is provided. Pagination enabled browsing of arrays and objects.


## Usage

    npm install big-json-viewer

## Demo

test.ts
```typescript
import { BigJsonViewer } from 'big-json-viewer';

document.body.appendChild(BigJsonViewer.elementFromData(JSON.stringify({
  test: 23,
  someArray: [45,2,5,true,false,null]
})));

```

index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Test</title>
  <link rel="stylesheet" href="./node_modules/big-json-viewer/styles/default.css">
</head>
<body>
<script src="src/test.ts"></script>
</body>
</html>
````

Example run with `parcel` (`npm install -D parce-bundler`);

    parcel index.html


## Getting started

You want to use the static method to display a JSON.

```typescript
BigJsonViewer.elementFromData(data: ArrayBuffer | string, options?: BigJsonViewerOptions): JsonNodeElement
```
    
It returns a `JsonNodeElement` object, that is an `HTMLDivElement` with some extras. You can insert it anywhere in your DOM.

You can also call some special methods on it like:

```typescript
node.openNode() // opens the root node, if it is an object or array
node.closeNode() // closes the node
node.toggleNode() // toggles the both above
node.openPath(path: string[]): boolean // opens the provided path and returns true, if the given path was found
node.openAll(maxDepth?: number, paginated?: PaginatedOption): number // opens all node under the given constraints
node.getOpenPaths(withStubs?: boolean): string[][] // gets a list of paths that are opened
```

## Options

When calling `elementFromData`, you can provide an object matching the interface `BigJsonViewerOptions`.

Example:

```javascript
{
  objectNodesLimit: 50, // how many properties of an object should be shows before it gets paginatated with a pagination size of 50
  arrayNodesLimit: 50, // same as objectNodesLimit, but with array elements
  labelAsPath: false // if true the label for every node will show the full path to the element
}
```

## Events

The following events are being fired on the visible DOM elements. The events bubble up, so you just need a listener to your root element.

#### openNode

Fires when a node is being opened. The target is a `JsonNodeElement`.

Can fire very often when Expand all button is pressed, depending on the structure.

Example logs the opened path:
```javascript
rootNode1.addEventListener('openNode', function(e) {
  console.log('openNode', e.target.jsonNode.path);
});
```

#### closeNode

Fires when a node is being closed. The target is a `JsonNodeElement`.

#### openStub

Fires when a pagination stub is being opened. The target is a `JsonNodesStubElement`.

Can fire very often when Expand all button is pressed, depending on the structure.

#### closeStub

Fires when a pagination stub is being closed. The target is a `JsonNodesStubElement`.

#### copyPath

Fires when the user clicks on the Copy Path link of a node.


## License

[MIT](LICENSE)



