# `bayes-probas` : Bayes classifier for node.js

Forked from https://www.npmjs.com/package/bayes, adds some functionnalities upon it (returning more informations when categorizing, unlearning)

`bayes` takes a document (piece of text), and tells you what category that document belongs to.

## What can I use this for?

You can use this for categorizing any text content into any arbitrary set of **categories**. For example:

- is an email **spam**, or **not spam** ?
- is a news article about **technology**, **politics**, or **sports** ?
- is a piece of text expressing **positive** emotions, or **negative** emotions?

## Installing

You'll need node 5.0+

```
npm install bayes-probas
```

## Usage

```
const bayes = require('bayes-probas')
const classifier = bayes()

// teach it positive phrases

classifier.learn('amazing, awesome movie!! Yeah!! Oh boy.', 'positive')
classifier.learn('Sweet, this is incredibly, amazing, perfect, great!!', 'positive')

// teach it a negative phrase

classifier.learn('terrible, shitty thing. Damn. Sucks!!', 'negative')

// unlearn something
classifier.learn('i hate mornings', 'positive');
... // uh oh, inadvertently associated 'i hate mornings' as being 'positive'.
classifier.unlearn('i hate mornings', 'positive');


// now ask it to categorize a document it has never seen before

classifier.categorize('awesome, cool, amazing!! Yay.')
// => 'positive'

// serialize the classifier's state as a JSON string.
let stateJson = classifier.toJson()

// load the classifier back from its JSON representation.
let revivedClassifier = bayes.fromJson(stateJson)
```

## API

### `var classifier = bayes([options])`

Returns an instance of a Naive-Bayes Classifier.

Pass in an optional `options` object to configure the instance. If you specify a `tokenizer` function in `options`, it will be used as the instance's tokenizer. It receives a (string) `text` argument - this is the string value that is passed in by you when you call `.learn()` or `.categorize()`. It must return an array of tokens. The default tokenizer removes punctuation and splits on spaces.

Eg.

```
let classifier = bayes({
    tokenizer: function (text) { return text.split(' ') }
})
```

### `classifier.learn(text, category)`

Teach your classifier what `category` should be associated with an array `text` of words.

### `classifier.unlearn(text, category)`

The classifier will unlearn the `text` that was associated with `category`.

### `classifier.categorize(text)`

Returns the `category` it thinks `text` belongs to. Its judgement is based on what you have taught it with `classifier.learn()`.
And an array of the categories sorted from most pertinent to less pertinent.

### `classifier.categorizeObj(text)`

Returns an array of `categories` ordered by likelihood to the `text` parameter.

The returned object is as such :
{
    probas,    //--->     [
                              {proba: logProbaCategoryA, probaH: humanReadableProbaCategoryA},
                              {proba: logProbaCategoryB, probaH: humanReadableProbaCategoryB}...
                          ]
    chosenCategory  //--> the main category bayes thinks the text belongs to. As a string
}

`probas[0].proba` = logarithmic likelihood of the most pertinent category
`probas[0].probaH` = likelihood on a scale from 0 to 100, 0 and 100. With 0 being the least likely category and 100 being the most likely.

### `classifier.toJson()`

Returns the JSON representation of a classifier.

### `let classifier = bayes.fromJson(jsonStr)`

Returns a classifier instance from the JSON representation. Use this with the JSON representation obtained from `classifier.toJson()`


## License

(The MIT License)

Original work Copyright (c) ttzel <tolgatezel11@gmail.com>
Modified work Copyright (c) Wozacosta  <wozacosta@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
