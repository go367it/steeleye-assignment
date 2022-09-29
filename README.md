# Frontend STEELEYE assignment

## Explaination of `List` component

---

<br/>

#### List is a functional component of React which uses hooks not lifecycle methods which are there in class components. It renders an unordered list and after getting the props ( `Items` ) from the parent element and pass it on to the `SingleListItem` to render.

<br/>

## Problems with the code

---

<br/>

- ### Inside the `List` component when rendering the `SingleListItem`, `key` is not given which will reduce the performance of the web app and it will make the rendering harder for the React

    ```
    <ul style={{ textAlign: 'left' }}>
        {items.map((item, index) => (
            <SingleListItem
            onClickHandler={() => handleClick(index)}
            text={item.text}
            index={index}
            isSelected={selectedIndex}
            />
        ))}
    </ul>
    ```
<br/>

- ### State is not defined properly inside the `List` component

    ### Wrong Code
    ```
    const [setSelectedIndex, selectedIndex] = useState();
    ```
    ### Correct Code
    ```
    const [selectedIndex, setSelectedIndex] = useState();
    ```

<br/>

- ### `.shapeOf()` is not a function in ProtoTypes instead use .`shape()` and use `PropTypes.checkPropTypes()` instead of `PropTypes.array()` because we are using new version of React which uses `prop-types` so it will give an error with `PropTypes.array()`

    ### Wrong Code
    ```
        WrappedListComponent.propTypes = {
            items: PropTypes.array(PropTypes.shapeOf({
                text: PropTypes.string.isRequired,
            })),
        };
    ```
    ### Correct Code
    ```
    WrappedListComponent.propTypes = {
        items: PropTypes.checkPropTypes(
            PropTypes.shape({
            text: PropTypes.string.isRequired
            })
        )
    };
    ```
<br />

## Correct React Code
---
<br/>

```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.checkPropTypes(
    PropTypes.shape({
      text: PropTypes.string.isRequired
    })
  )
};

WrappedListComponent.defaultProps = {
  items: null
};

const List = memo(WrappedListComponent);

export default List;
```
