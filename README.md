```jsx
import React from "react";
import { createSlice, configureStore } from "@reduxjs/toolkit";
import { Provider } from "react-redux";
import axios from "axios";
import { List, Button } from "antd";
import thunk from 'redux-thunk';

// Redux Slice
const readingMateSlice = createSlice({
name: "readingMate",
initialState: {
books: [],
readingHistory: [],
suggestions: [],
userProfile: {},
},
reducers: {
setBooks: (state, action) => {
state.books = action.payload;
},
setReadingHistory: (state, action) => {
state.readingHistory = action.payload;
},
setSuggestions: (state, action) => {
state.suggestions = action.payload;
},
setUserProfile: (state, action) => {
state.userProfile = action.payload;
},
},
});

export const { setBooks, setReadingHistory, setSuggestions, setUserProfile } = readingMateSlice.actions;

const store = configureStore({
reducer: readingMateSlice.reducer,
middleware: [thunk]
});

// Async actions
export const fetchBooks = () => async dispatch => {
const response = await axios.get("/api/books");
dispatch(setBooks(response.data));
};

export const fetchReadingHistory = () => async dispatch => {
const response = await axios.get("/api/readingHistory");
dispatch(setReadingHistory(response.data));
};

export const fetchSuggestions = () => async dispatch => {
const response = await axios.get("/api/suggestions");
dispatch(setSuggestions(response.data));
};

export const fetchUserProfile = () => async dispatch => {
const response = await axios.get("/api/userProfile");
dispatch(setUserProfile(response.data));
};

// Components
function BookList() {
React.useEffect(() => {
store.dispatch(fetchBooks());
}, []);

const books = store.getState().books;
return (
<List
itemLayout="horizontal"
dataSource={books}
renderItem={item => (
<List.Item>
<List.Item.Meta title={item.title} description={item.author} />
<Button>Read</Button>
</List.Item>
)}
/>
);
}

function ReadingMateApp() {
return (
<Provider store={store}>
<BookList />
</Provider>
);
}

export default ReadingMateApp;
```
--->
