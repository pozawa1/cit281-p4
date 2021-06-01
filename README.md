# Objectives
1. Gain experience writing and executing Node.js REST API server using VSCode
2. Gain experience using Fastify with the GET verb, routes, and route parameters
3. Gain experience using Postman to test web server routes
4. Gain experience working with JSON
5. Extra credit: Gain experience using Fastify with POST, PUT, and DELETE verbs

# Technologies Used
- Terminal
- VSCode
- Postman

# Part 1
Complete [Lab 5](https://pozawa1.github.io/cit281-lab5/)

# Part 2
Create the following files:
- p4-data.js: Data file
- p4-module.js: Code module that exports the question and answer functionality
- p4-server.js: Question and answer web server 
- package.json : Node.js configuration file

# Part 3
Create the data file, p4-data.js.

```
// Question and answer data array
const data = [
  {
    question: "Q1",
    answer: "A1",
  },
  {
    question: "Q2",
    answer: "A2",
  },
  {
    question: "Q3",
    answer: "A3",
  },
];

// Export statement must be below data declaration - no hoisting with const
module.exports = {
  data,
};
```

# Part 4
The code module file, p4-module.js, will import the data file, p4-data.js.

```
const { data } = require('./p4-data.js');
```

# Part 5
The code module file, p4-module.js, will contain the following six functions:
(1) getQuestions(): Returns an array of strings where each array element is a question.

```
function getQuestions() {
    return data.map(obj => obj.question);
};
```

(2) getAnswers(): Returns an array of strings where each array element is an answer.

```
function getAnswers() {
    return data.map(obj => obj.answer);
};
```

(3) getQuestionsAnswers(): Returns a copy of the original data array of objects.

```
function getQuestionsAnswers() {
    return [...data];
};
```

(4) getQuestion(number): Returns an object with the following properties: question (string), number (integer), and error message (string).

```
function getQuestion(number) {
    let index = number - 1;
    let newData = data[index];
    
    if (number <= 0) {
      return {
          question: '',
          number: '',
          error: 'Question number must be >= 1'
        }
    }
    if (number >= 4) {
      return {
          question: '',
          number: '',
          error: 'Question number must be less than the number of questions (3)'
      }
    }
    if (number >= 1 && number <= 3) {
      return {
          question: newData.question,
          number: number,
          error: ''
        }
    }
    else {
      return {
          question: '',
          number: '',
          error: 'Question number must be an integer'
        }
    }
};
```

(5) getAnswer(number): Returns an object with the following properties: answer (string), number (integer), and error message (string).

```
function getAnswer(number) {
  let index = number - 1;
  let newData = data[index];

  if (number <= 0) {
    return {
        answer: '',
        number: '',
        error: 'Answer number must be >= 1'
      }
  }
  if (number >= 4) {
    return {
        answer: '',
        number: '',
        error: 'Answer number must be less than the number of questions (3)'
      }
  }
  if (number >= 1 && number <= 3) {
    return {
        answer: newData.answer,
        number: number,
        error: ''
      }
  }
  else {
    return {
        answer: '',
        number: '',
        error: 'Answer number must be an integer'
      }
  }
};
```

(6) getQuestionAnswer(number): Returns an object with the following properties: question (string), answer (string), number (integer), and error message (string).

```
function getQuestionAnswer(number) {
  let index = number - 1;
  let newData = data[index];

  if (number <= 0) {
    return {
        question: '',
        answer: '',
        number: '',
        error: 'Question number must be >= 1'
      }
  }
  if (number >= 4) {
    return {
        question: '',
        answer: '',
        number: '',
        error: 'Question number must be less than the number of questions (3)'
      }
  }
  if (number >= 1 && number <= 3) {
    return {
        question: newData.question,
        answer: newData.answer,
        number: number,
        error: ''
      }
  }
  else {
    return {
        question: '',
        answer: '',
        number: '',
        error: 'Question number must be an integer'
      }
  }
};
```

# Part 6
Export the previous six functions.

```
module.exports = {getQuestions, getAnswers, getQuestionsAnswers, getQuestion, getAnswer, getQuestionAnswer, addQuestionAnswer, updateQuestionAnswer, deleteQuestionAnswer};
```

# Part 7
The REST API server file, p4-server.js, will respond to requests from Postman to routes that will test the exported functions from p4-module.js.

```
// Question Route
fastify.get("/cit/question", (request, reply) => {
    reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(
        {
            "error": "",
            "statusCode": 200,
            "questions": getQuestions()
        })
});

// Answer Route
fastify.get("/cit/answer", (request, reply) => {
    reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(
        {
            "error": "",
            "statusCode": 200,
            "answers": getAnswers()
        })
});

// Question and Answer Route
fastify.get("/cit/questionanswer", (request, reply) => {
    reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(
        {
            "error": "",
            "statusCode": 200,
            "questions_answers": getQuestionsAnswers()
        })
});

// Specific Question Route
fastify.get("/cit/question/:number", (request, reply) => {
    let number = request.params.number;
    let newQ = getQuestion(number);

    reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(
        {
            "error": newQ.error,
            "statusCode": 200,
            "question": newQ.question,
            "number": newQ.number
        })
});

// Specific Answer Route
fastify.get("/cit/answer/:number", (request, reply) => {
    let number = request.params.number;
    let newA = getAnswer(number);

    reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(
        {
            "error": newA.error,
            "statusCode": 200,
            "question": newA.answer,
            "number": newA.number
        })
});

// Specific Question and Answer Route
fastify.get("/cit/questionanswer/:number", (request, reply) => {
    let number = request.params.number;
    let newQA = getQuestionAnswer(number);

    reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(
        {
            "error": newQA.error,
            "statusCode": 200,
            "question": newQA.question,
            "answer": newQA.answer,
            "number": newQA.number
        })
});

// Unmatched Route
fastify.get("*", (request, reply) => {
    reply
      .code(200)
      .header("Content-Type", "application/json; charset=utf-8")
      .send(
        {
            "error": "Route not found",
            "statusCode": 404
        });
  });
```

# Part 8
Add support for POST.

```
function addQuestionAnswer(info = {}) {
  if (Object.keys(info).length === 0 || "question" in info === false) {
    return {
      error: "Object question property required",
      message: "",
      number: -1
    }
  }
  if ("answer" in info === false) {
    return {
      error: "Object answer property required",
      message: "",
      number: -1
    }
  }
  if ("question" in info === true && "answer" in info === true) {
    let addQA = data.push(info);
    return {
      error: "",
      message: "Question added",
      number: addQA
    }
}};
```

```
fastify.post("/cit/question", (request, reply) => {
    let response = request.body;
    let addedQA = addQuestionAnswer(response);
    
    reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(
    {
        "error": addedQA.error,
        "statusCode": 201,
        "number": addedQA.number
    });
});
```

# Part 9
Add support for PUT.

```
function updateQuestionAnswer(info = {}) {
  if (Object.keys(info).length === 0) {
    return {
      error: "Object question property or answer property required",
      message: "",
      number: ""
    }
  }
  if ("number" in info === false) {
    return {
      error: "Object number property must be a valid integer",
      message: "",
      number: ""
    }
  }
  if ("number" in info === true ) {
    let index = info.number - 1;
    data[index].question = info.question;
    data[index].answer = info.answer;
    
    return {
      error: "",
      message: "Question" + " " + info.number + " " + "updated",
      number: info.number
    }
}};
```

```
fastify.put("/cit/question", (request, reply) => {
    const { number, question, answer } = request.body;
    let response = { number, question, answer };
    let updatedQA = updateQuestionAnswer(response);
    
    reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(
    {
        "error": updatedQA.error,
        "statusCode": 200,
        "number": updatedQA.number
    });
});
```

# Part 10
Add support for DELETE.

```
function deleteQuestionAnswer(info) {
  let index = info - 1;
  
  if (info <= 0) {
    return {
      error: "Question/answer number must be >= 1",
      message: "",
      number: ""
    }
  }
  if (info >= 4) {
    return {
      error: "Question/answer number must be less than the number of questions",
      message: "",
      number: ""
    }
  }
  if (info >= 1 && info <= 3) {
    delete data[index];

    return {
      error: "",
      message: "Question" + " " + info + " " + "deleted",
      number: info
    }
  }
  else {
    return {
      error: "Question/answer number must be an integer",
      message: "",
      number: ""
    }
}};
```

```
fastify.delete("/cit/question/:number", (request, reply) => {
    let number = request.params.number;
    let response = number;
    let deletedQA = deleteQuestionAnswer(response);
    
    reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(
    {
        "error": deletedQA.error,
        "statusCode": 200,
        "number": deletedQA.number
    });
});
```

[Source Code](https://github.com/pozawa1/cit281-p4/blob/main/source-code-p4)

[Return to Homepage](https://pozawa1.github.io/)
