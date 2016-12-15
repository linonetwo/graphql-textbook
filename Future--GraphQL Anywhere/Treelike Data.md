
```javascript
const query = gql`
  {
    div {
      s1: span(id: "my-id") {
        text(value: "This is text")
      }
      s2: span
    }
  }
`;

assert.equal(
  renderToStaticMarkup(gqlToReact(query)),
  '<div><span id="my-id">This is text</span><span></span></div>'
);
```


```javascript
{ dir(path: "./test/"){ files { name }, subdirectories { name } } }
```

```javascript
{ data: 
      { dir: 
        { 
          files: [
            { name: 'babel-index.js' },
            { name: 'index.js' },
            { name: 'sample.txt' },
          ],
          subdirectories: [
            { name: 'uga' },
          ]
        },
      } 
    }
```