---
tags:
  - "2023"
  - OpenAI
  - Fun projects
comments: true
---

# novela.ink - AI assistant for creators


![novela.ink](../assets/logo-3.png)


[GitHub repository](https://github.com/razikus/novela)

[LabLab.ai link](https://lablab.ai/event/openai-whisper-gpt3-codex-dalle2-hackathon/approxteam/novela-ink)


Youtube video:

[![Watch the video](https://img.youtube.com/vi/e14Tk476YOM/hqdefault.jpg)](https://www.youtube.com/watch?v=e14Tk476YOM)  
## OpenAI hackathon

Recently I found out very interesting website https://lablab.ai which is organising and planning a lot of AI hackathons.

It's very good opportunity to check and touch new technologies and im treating it a bit as a sport.

Purpouse of this particular hackathon was to create a soluton that uses existing OpenAI tools like GPT-3 or Dalle.

## Idea

My idea was preety simple - create a software that can help to create books. It has mainly some features:

- Story completion (also in-place between 2 texts, so generates inside missing text)
- Image generation for chapter summary
- Undo / Redo
- Saving, deletion
- Inspirations (AI helps to get something related to your prompt)

## Simplified Architecture

![Architecture](../assets/novela.drawio.png)

Architecture is pretty simple and preety obvious. Using OpenAI also is very simple.

- Novela - backend worker
- Dalle - AI prompt 2 image provider
- GPT-3 - language model provider (text-davinci-003)
- immudb - as immutable database (has undo redo feature built-in)
- Frontend - frontend :)

## Used technology

- Backend - [FastAPI](https://fastapi.tiangolo.com/) 
- Frontend - [Quasar.js](https://quasar.dev/)  
- Database - [immudb](https://immudb.io/)
- OpenAI API - [OpenAI](https://beta.openai.com/docs/introduction)

Of course dockerized and easy to deploy


## Some interesting code parts

```py
    def completeStory(self, forWhat, temperature = 0.3, max_tokens=2048, suffix = None, action = "", n = 1):
        forWhat = f"Find a {action} continue for:\n" + forWhat
        req = {
            "model": self.model,
            "prompt": forWhat,
            "max_tokens": max_tokens,
            "temperature": temperature,
            "n": n
        }
        if suffix:
            req["suffix"] = suffix
        completed = self.client.create(**req)
        return 
```

With novela you can make few story completions (full of action, funny, etc).

Like you can see - the prompt itself instruct GPT to complete the prompt.

Suffix parameter indicates that result of the prompt should continue as suffix, so completion should fit between prefix and suffix.


To generate images from summary:

```py
class AiSummarizer:

    def __init__(self, api_key):
        self.api_key = api_key
        self.model = "text-davinci-003"
        self.client = openai.Completion()

    def whatIsThatAbout(self, content, max_tokens=256):
        req = {
            "model": self.model,
            "prompt": "Generate a short one sentence summary that could be applicable as DALEE2 input in english that has max 12 words, about this: \n" + content,
            "max_tokens": max_tokens,
            "temperature": 0.8,
            "n": 1
        }
        answer = self.client.create(**req)
        return answer
```

Like you can see here - I just used GPT-3 to generate me an input for DALEE.

## Thoughts

During implementation and making the project I had several predictions for the future and several thoughts that I will try to list here.

- AI will becase normal-day assistant soon.
- Everyone will use more complex AI novadays (not only to make beautiful instagram picture)
- New branch of programming - prompt engineering - choosing a best prompt was the most challenge here to be honest. Combinations, choosing proper sequence of words.
- It's still a big model. (Are we a big models also?)
- Cannot be yet used for decision making - mainly because world is changing every day and currently model is trained up to some point.
- In future software would have assistants built-in.
- It was very easy to implement.


## Prizes and what next

Finaly I've got 3rd place, 250$ Digital Ocean credits, and this cool certificate!

And of course - hands on technology.

![Certificate](../assets/cert.png)

What next with that? Some people contacted me, let's see if we can do something.