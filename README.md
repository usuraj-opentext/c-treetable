# Angular Material TreeTable Component

[![Build Status](https://travis-ci.com/mlrv/ng-material-treetable.svg?branch=master)](https://travis-ci.com/mlrv/ng-material-tree-table) 
[![npm version](https://d25lcipzij17d.cloudfront.net/badge.svg?id=js&type=6&v=1.0&x2=0)](https://badge.fury.io/js/ng-material-treetable)


A simple, customisable, and easy to use Angular Material TreeTable component with Custom Template injection.

This is based on https://www.npmjs.com/package/ng-material-treetable

---

## Table of Contents

1. [Installation](#installation)
1. [Data Format](#data-format)
1. [Options](#options)
1. [Events](#events)

## Installation

Simply install the package through `npm`

```
npm i c-treetable --save
```

Make sure you have the angular material packages installed

```
npm i @angular/material @angular/cdk @angular/animations --save
```

import the main module

```typescript
import { TreetableModule } from 'c-treetable';

@NgModule({
    ...
  imports: [
    ...
    TreetableModule
  ],
  ...
})
export class AppModule { }
```

and use the component in your template

```html
<c-treetable [tree]="yourTreeDataStructure"></c-treetable>
```

Finally, make sure you import the required material icons font in your `styles.css`

```css
@import url('https://fonts.googleapis.com/css?family=Roboto:400,700|Material+Icons');
```

## Data Format

The tree object that's rendered by the component can either be a `Node<T>` or a `Node<T>[]` where `Node<T>` is the following interface

```typescript
import { Node } from 'c-treetable';

interface Node<T> {
  value: T;
  children: Node<T>[];
}
```

Here's a simple example.


```javascript
{
  value: {
    name: 'Reports',
    owner: 'Eric',
    protected: true,
    backup: true
  },
  children: [
    {
      value: {
        name: 'Charts',
        owner: 'Stephanie',
        protected: false,
        backup: true
      },
      children: []
    },
    {
      value: {
        name: 'Sales',
        owner: 'Virginia',
        protected: false,
        backup: true
      },
      children: []
    },
    {
      value: {
        name: 'US',
        owner: 'Alison',
        protected: true,
        backup: false
      },
      children: [
        {
          value: {
            name: 'California',
            owner: 'Claire',
            protected: false,
            backup: false
          },
          children: []
        },
        {
          value: {
            name: 'Washington',
            owner: 'Colin',
            protected: false,
            backup: true
          },
          children: [
            {
              value: {
                name: 'Domestic',
                owner: 'Oliver',
                protected: true,
                backup: false
              },
              children: []
            },
            {
              value: {
                name: 'International',
                owner: 'Oliver',
                protected: true,
                backup: true
              },
              children: []
            }
          ]
        }
      ]
    }
  ]
}
```

## Options

> Work in Progress...

An `option` input property can be used to customise the component

```typescript
import { Node, Options } from 'c-treetable';
```

---

```html
<treetable
  [tree]="yourTreeDataStructure"
  [options]="yourOptions">
</treetable>
```

| Name                  | Description                                                                                                                                                                                                                                                                                | Type      | Default |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|---------|
| `verticalSeparator`   | If `true`, separates table columns with  vertical lines.                                                                                                                                                                                                                                   | `boolean` | `true`  |
| `capitalisedHeader`   | If `true`, capitalise the first letter of each column header.                                                                                                                                                                                                                              | `boolean` | -       |
| `highlightRowOnHover` | If `true`, hovering the mouse over a row highlights its background.                                                                                                                                                                                                                        | `boolean` | `true`  |
| `customColumnOrder`   | By default, the columns are ordered following  the array generated by calling `Object.keys()` on the nodes of the tree object; this option  can be used to specify a custom order. Note that `customColumnOrder` needs to be an  array containing all the keys present in the node object. | `Array`   | -       |
| `elevation`           | Sets the elevation of the card element wrapping the tree component.                                                                                                                                                                                                                        | `number`  | `5`  |

## Templating

By default name of a node is displayed inside a tree node, in case you need to place custom content define a Template (ng-template) that gets the treenode as an implicit variable. 

Example below places an input field to create editable treenodes.

```html
<treetable [tree]="arrayOfNodesTree" [body]="myElement">
</treetable>

<ng-template #myElement let-obj>
    <div>custom content</div>
</ng-template>
```
The default behaviour is as follows : 
```html
<!-- Default Body Element -->
<ng-template #defaultBodyElement let-el>
  <!-- 
    el : {
           element : any ,
           column : any 
         }
  -->
    <div>{{el.element.value[el.column]}}</div>
</ng-template>
```


---


### customColumnOrder

Given a tree data type like

```typescript
interface Person {
  name: string;
  age: number;
  married: boolean;
}

const tree: Node<Person> = ...
```

a custom column order can be specified with the following `options` object

```typescript
const opts: Options<Person> = {
  customColumnOrder: ['married', 'age', 'name']
}
```

an incomplete or incorrect `customColumnOrder` value will result in an error

```typescript
customColumnOrder: ['married', 'age'] // 'name' missing
customColumnOrder: ['married', 'age', 'name', 'surname'] // 'surname' is not a valid key
```



## Events

> Work in Progress...

| Name          | Description                                                                              | Type      |
|---------------|------------------------------------------------------------------------------------------|-----------|
| `nodeClicked` | Whenever a node is expanded or collapsed, emits an event with the new status of the node | `Node<T>` |

### nodeClicked

```html
<treetable
  [tree]="yourTreeDataStructure"
  (nodeClicked)="logToggledNode($event)">
</treetable>
```

---

```typescript
logToggledNode(node: Node<SomeNodeType>): void {
  console.log(node);
}
```
