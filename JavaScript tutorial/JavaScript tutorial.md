# JavaScript tutorial

## 1) JavaScript first steps

The three layers build on top of one another nicely. Let’s take a simple text label as an example. We can mark it up using HTML to give it structure and purpose:

```
<p>Player 1: Chris</p>
```

Then we can add some CSS to into the mix to get it looking nice:

```
p {
  font-family: "helvetica neue", helvetica, sans-serif;
  letter-spacing: 1px;
  text-transform: uppercase;
  text-align: center;
  border: 2px solid rgba(0, 0, 200, 0.6);
  background: rgba(0, 0, 200, 0.3);
  color: rgba(0, 0, 200, 0.6);
  box-shadow: 1px 1px 2px rgba(0, 0, 200, 0.4);
  border-radius: 10px;
  padding: 3px 10px;
  display: inline-block;
  cursor: pointer;
}
```

And finally, we can add some JavaScript to implement dynamic behavior:

```
const para = document.querySelector('p');

para.addEventListener('click', updateName);

function updateName() {
  const name = prompt('Enter a new name');
  para.textContent = `Player 1: ${name}`;
}
```

![Styled paragraph of Player 1: Chris](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript/html-and-css.png)