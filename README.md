# Rounabh Sahu_Front End

**1.Explain what the simple List component does.**
From the partially correct code as I can see, it looks like it shows an item of the list (mapped from the parent). Uses isSelected bool to check whether the selected item is the one that has the same index with the one that is selected: if true, make it green; otherwise, make it red. Also, note it uses memo, that means if the prop passed to the simple list component doesn't change, it won't re-render. That means a boost in performance by reducing unnecessary re-renders.

**2.What problems / warnings are there with code?**

-    ` const [setSelectedIndex, selectedIndex] = useState(); ` is not correct as useState returns the first element of the value, then the next one is the function that will be used to set, but in the code, it is reversed.
-     `PropTypes.array(PropTypes.shapeOf({ `is an incorrect syntax. It should be `PropTypes.array(PropTypes.shape({`.
-    ` isSelected={selectedIndex} `As per my understanding, a boolean value should be passed to the prop like this: `{selectedIndex === index}`.
-     `onClick={onClickHandler(index)} `should be `onClick={() => onClickHandler(index)}`.

**3. Please fix, optimize, and/or modify the component as much as you think is necessary.**

```
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const SingleListItem = memo(({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red' }}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
});

SingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

// List Component
const List = memo(({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
});

List.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ).isRequired,
};

export default List;
```

improvements :
fixed errors as mentioned above
improved performance with index passed on single list component

`List.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ).isRequired,
}; `
