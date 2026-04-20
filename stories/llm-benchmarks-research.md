# LLM Benchmarks Research

Claude helped me track down two YouTube videos and websites I half-remembered from recent watching -- one about LLMs failing a deceptively simple common-sense question, and another about testing whether models can spot outright nonsense.

## The Car Wash Test

A viral LLM benchmark where models are asked: *"I want to wash my car. The car wash is 50 meters away. Should I walk or drive?"* Most models fail by saying "walk" -- the short-distance heuristic overrides the obvious constraint that the car needs to be at the car wash.

[Opper.ai](https://opper.ai/blog/car-wash-test) tested 53 models. Only 5 passed consistently across 10 runs: Claude Opus 4.6, Gemini 2.0 Flash Lite, Gemini 3 Flash, Gemini 3 Pro, and Grok-4. An expanded study at [CarWashBench](https://github.com/CarWashBench/CarWashBench) tested 131 models and found 76% still get it wrong. There are also academic papers on the phenomenon: a [variable isolation study](https://arxiv.org/html/2602.21814v1) and a paper on how [surface heuristics override implicit constraints](https://arxiv.org/html/2603.29025v1).

## BullshitBench -- Can AI Spot Nonsense?

[BullshitBench](https://github.com/petergpt/bullshit-benchmark) measures whether AI models challenge nonsensical prompts instead of confidently answering them. Peter Gostev's YouTube video ["Can AI spot nonsense? We tested 80 models -- thinking ones did worst"](https://www.youtube.com/watch?v=bOLXvFMqhi8) walks through the results. The counterintuitive finding: reasoning-focused models performed *worse* because they try harder to find meaning in meaningless prompts.

## SimpleBench

A related benchmark at [simple-bench.com](https://simple-bench.com/), created by the "AI Explained" YouTube channel. 200+ trick questions covering spatial reasoning, social intelligence, and linguistic adversarial robustness. Humans score ~84%. Top LLMs score under 42%.

**What Claude did for me:** I described two half-remembered videos from my YouTube history -- one about non-sequitur questions and a tracking website, another about walking vs. driving to a car wash. Claude identified both topics, found the relevant websites, GitHub repos, academic papers, and YouTube videos in about five minutes of web searching.
