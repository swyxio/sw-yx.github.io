---
layout: postk
date: 2018-07-11
tags: babel graphql
feelings: neutral
title: fragments for babel-blade
comments: true
description: a gnarly problem
---

babel-blade is coming along nicely, but i have a pretty gnarly problem with fragments.

see astexplorer: http://astexplorer.net/#/gist/4b72d63ecd01237e179c102f6df9c2b4/dd3f21d5ac100a7a045e6329750d64d4a4cf7f8d

devon wants an implicit API with some magic. i want less magic but cant figure out how to implement it.

currently this:

```jsx
import { Connect, query } from 'urql';
import {createFragment, createQuery} from 'blade.macro';

const movieFragment = createFragment('MovieType');
const Movie = ({data}) => {
  let result = movieFragment(data);
  let movie = result.movie;
  return (
    <div className="movie">
      {loaded === false ? (
        <p>Loading</p>
      ) : (
        <div>
          <h2>{movie.test.title}</h2>
          <p>{movie.test.chimp.field}</p>
          <p>{movie.test.chimp.jank}</p>
          <p>{movie.test.chimp.doop}</p>
          <p>{movie.foo}</p>
          <button onClick={onClose}>Close</button>
        </div>
      )}
    </div>
  );
}

Movie.fragment = (name) => movieFragment;

const pageQuery = createQuery(); // create a top-level query
const App = () => (
  // rendering Movie automatically composes `Movie.fragment` into the query.
  <Connect
    query={query(pageQuery)}
    children={({ loaded, data }) => {
      let result = pageQuery(data);
      return <Movie data={result.movie(null, Movie.fragment)} />;
    }} />
);
```

generates

```jsx
import { Connect, query } from 'urql';
const Movie = ({data}) => {
  let result = data;
  let movie = result.movie;
  return (
    <div className="movie">
      {loaded === false ? (
        <p>Loading</p>
      ) : (
        <div>
          <h2>{movie.test.title}</h2>
          <p>{movie.test.chimp.field}</p>
          <p>{movie.test.chimp.jank}</p>
          <p>{movie.test.chimp.doop}</p>
          <p>{movie.foo}</p>
          <button onClick={onClose}>Close</button>
        </div>
      )}
    </div>
  );
}

Movie.fragment = (name) => `
fragment movieFragment on MovieType{
  movie {
    test {
      title
      chimp {
        field
        jank
        doop
      }
    }
    foo
  }
}`;

const App = () => (
  // rendering Movie automatically composes `Movie.fragment` into the query.
  <Connect
    query={query(`
query pageQuery{
  fragment {
    movie_2df9: movie(${null})
  }
}`)}
    children={({ loaded, data }) => {
      let result = data;
      return <Movie data={result.movie_2df9} />;
    }} />
);
```

apart from the null argument getting picked up which i can fix, i need to pass in the fragment name AND the fragment query itself to the  top level query. i think i can do it but it will take some finagling and maybe not look very pretty.

the key is for Movie.fragment to be a function instead of just a template string.
