## React-Query useQuery setup with axios and Context API

This code snippet helps setup react-query to fetch data with axios from a GET request endpoint, and also pass the data to children components with React Context API.

Note: The entire app must be wrapped in QueryClientProvider for react-query to work. this will typically be done at root level in react, i.e App.js or index.js


```js
import React, { Fragment, useState } from "react";
import { Switch, Route, useRouteMatch } from "react-router-dom";
import Navbar from "../shared/components/Navbar";
import Home from "./Home";
import Post from "./Post";
import { useQuery } from "react-query";
import axios from "axios";
import { QueryClient, QueryClientProvider } from "react-query";

const queryClient = new QueryClient();

export const PostContext = React.createContext();

const SampleComponent = () => {
  const { path } = useRouteMatch();

// Define function to fetch data with axios
  const getCode = async () => {
    const { data } = await axios.get(
      "http://192.168.0.149:1234/data"
    );
    return data;
  };

// Use useQuery from React-Query to automatically handle the data fetching, refetching, loading states etc.

  const { isLoading, status, error, data, isFetching } = useQuery(
    ["post", limit, filter],
    () => getCode(limit, filter)
  );

  return (
	<QueryClientProvider client={queryClient}>
    <PostContext.Provider
      value={{
        isLoading,
        status,
        error,
        data,
        limit,
        setFilter,
        setLimit,
        isFetching,
      }}
    >
      <Navbar />
      <Switch>
        <Route exact path={"/:post_id"} component={Post} />
        <Route path={"/"} component={Home} />
      </Switch>
    </PostContext.Provider>
	</QueryClientProvider>
  );
};

export default SampleComponent;
``` 
