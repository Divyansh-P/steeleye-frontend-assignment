# Steeleye-frontend-assignment
## 1. Explain what the simple List component does.
* The List component generates an unordered list with given prop items where initially the background color of the list item is red and whenever you click on a list item it gets selected and its background color is changed to green.
## 2. What problems/warnings are there with code?
* Arrow function should be used in onClick event in WrappedSingleList component.
```
<li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}
    >
```

   **correction**
```
 <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={()=>onClickHandler(index)}
    >
```
* Syntax error in useState hook in WrappedListComponent.
```
const [setSelectedIndex, selectedIndex] = useState();
```
   **correction**
```
  const [selectedIndex,setSelectedIndex] = useState();
```
* Syntax error in WrappedListComponent.propTypes.
```
WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};
```
   **correction**
```
  WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};
```
* default props are set to null they should have some default value to render initial testing list.
```
WrappedListComponent.defaultProps = {
  items: null,
};
```
   **correction**
```
WrappedListComponent.defaultProps = {
  items:[
    {
      text:"This is list no. 1"
    },
    {
      text:"This is list no. 2"
    },  {
      text:"This is list no. 3"
    },  {
      text:"This is list no. 4"
    },
  ]
};
```

* isSelected is of bool type but we are returning number value.
* key should be added to every list component that we are generating.


## 3. Please fix, optimize, and/or modify the component as much as you think is necessary.
**Optimised/fixed Code**
```

import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={()=>onClickHandler(index)}
    >

      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index:PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {

  const [selectedIndex,setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
        index={index}
        key={index}
          onClickHandler={()=>handleClick(index)}
          text={item.text}
          isSelected={selectedIndex===index}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
  items:[
    {
      text:"This is list no. 1"
    },
    {
      text:"This is list no. 2"
    },  {
      text:"This is list no. 3"
    },  {
      text:"This is list no. 4"
    },
  ]
};

const List = memo(WrappedListComponent);

export default List;


```
