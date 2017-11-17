To run a gitbook locally, run 

```
npm install gitbook-cli -g

cd Whitepaper # (or whatever book you are working on)
gitbook install
gitbook serve
```

Then point your browser at `localhost:4000`

**Note** `book.json` installs a `Mathjax` plugin that enables LaTex rendering wtihin markdown with the usual double dollarsign.
$$This will be rendered with LaTex$$
