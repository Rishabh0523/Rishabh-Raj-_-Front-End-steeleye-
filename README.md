ASSIGMENT SOLUTIONS :-

( 1 )  Explain what the simple List component does.
Ans : The simple List component typically consists of a container element that holds a series of smaller item elements, each of which represents a specific piece of information or action. The items can be text, images, or icons, and they can be organized in a specific order or sorted based on a specific criterion.

In websites, occasionally we want our data to be displayed in ordered format (as it is quite more proficient) and this can be done by using List.
Overall, the simple List component is a versatile and commonly used user interface element that provides an intuitive way for users to view and interact with data in a structured and organized manner.
In the below code, List component do the following things :

•	It consists of List Items wrapped in WrappedSingleListItem having state isSelected and values as text and index, and event named onClickHandler, on which further, these are memoized under memo(WrappedListComponent) which basically remembers the arguments that has been passed with the results and if it is passed with same arguments, then thus it will return the result directly from memory.

•	It uses map() method, to call a function for each element in that array, and it stores the listed items under array items.
And side by side the background color of the passed text is changing with respect to  their state, like if state is true(selected list items) it will show green and red for non selected items.

The simple List component often includes features like scrolling, filtering, searching, and pagination, which make it easier for users to navigate through a large set of data. Additionally, the simple List component can be customized to fit the specific needs and branding of a particular application or website.


( 2 ) What problems / warnings are there with code?
ANS : 

1 . While using useState() the 'set' variable should be after the local variable.

Fixed code:
const [selectedIndex, setSelectedIndex] = useState();


2. Syntax error of PropTypes in WrappedListComponent. It should be arrayOf instead of shapeOf.

Fixed code:
WrappedListComponent.propTypes = {
    items: PropTypes.arrayOf(
        PropTypes.shape({
            text: PropTypes.string.isRequired
        })
    )
};

3. Error in declaring selectedIndex, it was throwing an error of React Hook useEffect has a missing dependency: 'setSelectedIndex'

            Error:
            const [setSelectedIndex, selectedIndex] = useState();

            Correction:
            const [selectedIndex, setSelectedIndex] = useState(false);

4.  As map() method was called and by default it was declared it as null due to which it is throwing an error. Ideally data should be passed from App component but in this case as data is not passed from App component so we can pass data as default prop

          Error:
          WrappedListComponent.defaultProps = {
                 items: null,
             };

          Correction:
          WrappedListComponent.defaultProps = {                   
            items: [
            { text: "Rishabh Raj" },
            { text: "12006056” },
            { text: "BTech CSE (2024)"},
            { text: "Lovely Professional University”}
            ],
            };

5.  onClickHandler is basically being called as a function here which is why rendered color is always green as it takes value of index initially passed always, instead it should be called as a arrow function.

            Error:
            onClick={onClickHandler(index)}

            Correction :
            onClick={()=>onClickHandler(index)}

6.         Error:    
            const handleClick = index => { setSelectedIndex(index); };

            Correction:
            const handleClick = index => { setSelectedIndex(true) };

3. fix, optimize, and/or modify the component as much as you think is necessary.

Code :
This is the fix and the optimize code : 

import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
	return (
		<li
			style={{ backgroundColor: isSelected ? "green" : "red" }}
			onClick={onClickHandler(index)}>
			{text}
		</li>
	);
};



WrappedSingleListItem.propTypes = {
	index: PropTypes.number,
	isSelected: PropTypes.bool,
	onClickHandler: PropTypes.func.isRequired,
	text: PropTypes.string.isRequired,
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
	items: PropTypes.arrayOf(
		PropTypes.shape({
			text: PropTypes.string.isRequired,
		})),
};

WrappedListComponent.defaultProps = {
	items: [{text: " Rishabh Raj "}, 
  {text: "12006056"},
  { text:"BTech CSE (2024)"},
  {text: "Lovely Professional University"},
],
};

const List = memo(WrappedListComponent);

export default List;


<img width="1427" alt="Screenshot 2023-04-23 at 4 58 13 PM" src="https://user-images.githubusercontent.com/76212232/233837760-ae89017f-e5c4-4758-9379-596d43e7a0f4.png">
