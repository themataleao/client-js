This javascript client is inspired from [cohere-typescript](https://github.com/cohere-ai/cohere-typescript)

# Mistral Javascript Client

You can use the Mistral Javascript client to interact with the Mistral AI API.

## Installing

You can install the library in your project using:

`npm install @mistralai/mistralai`

## Usage
### Set up
```typescript
import MistralClient from '@mistralai/mistralai';

const apiKey = "Your API key";

const client = new MistralClient(apiKey);
```

### List models
```typescript
const listModelsResponse = await client.listModels();
const listModels = listModelsResponse.data;
listModels.forEach((model) => {
  console.log('Model:', model);
});
```

### Chat with streaming
```typescript
const chatStreamResponse = await client.chatStream({
  model: 'mistral-tiny',
  messages: [{role: 'user', content: 'What is the best French cheese?'}],
});

console.log('Chat Stream:');
for await (const chunk of chatStreamResponse) {
  if (chunk.choices[0].delta.content !== undefined) {
    const streamText = chunk.choices[0].delta.content;
    process.stdout.write(streamText);
  }
}
```
### Chat without streaming
```typescript
const chatResponse = await client.chat({
  model: 'mistral-tiny',
  messages: [{role: 'user', content: 'What is the best French cheese?'}],
});

console.log('Chat:', chatResponse.choices[0].message.content);
```
###Embeddings
```typescript
const input = [];
for (let i = 0; i < 1; i++) {
  input.push('What is the best French cheese?');
}

const embeddingsBatchResponse = await client.embeddings({
  model: 'mistral-embed',
  input: input,
});

console.log('Embeddings Batch:', embeddingsBatchResponse.data);
```
## Run examples

You can run the examples in the examples directory by installing them locally:

```bash
cd examples
npm install .
```

### API Key Setup

Running the examples requires a Mistral AI API key.

1. Get your own Mistral API Key: <https://docs.mistral.ai/#api-access>
2. Set your Mistral API Key as an environment variable. You only need to do this once.

```bash
# set Mistral API Key (using zsh for example)
$ echo 'export MISTRAL_API_KEY=[your_key_here]' >> ~/.zshenv

# reload the environment (or just quit and open a new terminal)
$ source ~/.zshenv
```

You can then run the examples using node:

```bash
MISTRAL_API_KEY=XXXX node chat_with_streaming.js
```
