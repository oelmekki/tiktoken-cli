# tiktoken-cli

Simple wrapper around [tiktoken](https://github.com/openai/tiktoken) to use
it in your favorite language.

## Why

If you play with openAI's GPT API, you probably encounter one annoying
problem : your prompt is allowed a given amount of tokens, you have no
idea how that tokens are counted, and you only know it was too much when
the api replies with an error, which is seriously annoying.

OpenAI published its [tiktoken](https://github.com/openai/tiktoken) token
counting library to solve that, which helps a lot! If you write a python
program.

`tiktoken-cli` allows you to write your program in any language. It's a
simple wrapper around `tiktoken` that will read your prompt from STDIN and
write the amount of tokens to STDOUT. Now you can write your program in
your favorite language and use the CLI for token counting.

It's almost not worth publishing a github repos for so few lines, but I
figured that README explanation would be valuable for people wondering how
to use that API in their favorite language, the code is merely an
executable example.

A note for those of you who may be new to GPT's API : having the number of
tokens from your prompt alone is not enough to avoid the exceed context
errors. This is because the API wraps your messages with its own content.
The exact algorithm to know if your going to exceed the count is documented
[in the api doc here](https://platform.openai.com/docs/guides/chat) (it's
in the block "Deep dive : Counting tokens for chat API calls").

## Install

`tiktoken-cli` is a simple script, you can just download it and put it
where it's convenient for you.

Make it executable:

```shell
chmod +x ./tiktoken-cli
```

And install tiktoken:

```shell
pip install tiktoken
```

## Usage

To use `tiktoken-cli` send your prompt as STDIN, and read the token count
as STDOUT.

Examples:

In shell:

```shell
count=$(echo "Hello, world!" | ./tiktoken-cli)
echo $count # 4
```

In ruby:

```ruby
input_data = "Hello, world!"

output_data = IO.popen('./tiktoken-cli', 'r+') do |pipe|
  pipe.puts input_data
  pipe.close_write
  pipe.read
end

puts output_data # 4

# note that the value is a string, you need to cast it
# to use it as a number
puts output_data.to_i + 10
```

## Model

`tiktoken` counts tokens differently based on model. By default, the model
used is gpt-3.5-turbo-0301 (that's the chatGPT model, at time of writing).

You can change the model using the `TIKTOKEN_MODEL` environment variable.
Or, you know, edit the source file, it's 17 lines of code. :)
