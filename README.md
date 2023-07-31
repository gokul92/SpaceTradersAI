# SpaceTradersAI

## Introduction
An AI to play the SpaceTraders.io game

SpaceTraders is an open-universe, programmable fleet-management game played through a headless API. Thus, it can be entirely played by making GET, POST, PATCH calls to the endpoint.
You can trade goods across systems, mine asteroids for valuable ores, or take on faction contracts to earn credits and reputation.

## Documentation

You can learn more about the game here - https://spacetraders.io/

The documentation for the API, which was generated using OpenAPI is available here - https://docs.spacetraders.io/. 

## Goal
The goal of this work is to build an AI that can play the game in an automated fashion, once the user specifies a set of goals they want to achieve.

## Work in progress
The SpaceTraders-AI jupyter notebook currently does the following - 
1. Allows you to register your own SpaceTraders agent
2. Load, clean and reduce the SpaceTraders API documentation
3. Instantiate an LLM planning agent (OpenAI token required)
4. Feed the specs and prime the LLM agent
5. Create a query and ask the LLM agent to accomplish a set of tasks

## Limitations
The primary limitations are - 
1. The context length is greater than 4000 tokens but the combined prompt + completion length is limited to 4097 tokens. Hence, the LLM is unable to carry out complex tasks involving multiple steps and is limted to smaller tasks involving fewer API calls
2. The Chat model is not interactive so the entire documentation has to be fed in a single shot. 
3. There is no opportunity to answer any clarifying questions the agent might have. Hence, if any aspect of the goals are unclear, it fails.
4. Given the length of the documentation (over 50,000+ tokens), the LLM agent can lose context when trying to accomplish sophisticated goals involving multiple tasks. This can be minimized by making the chat interactive and priming the LLM with appropriate portions of the documentation when in doubt.

## Next steps
1. Make the chat model interactive. This will allow us to break the tasks into pieces, work around the context length limit and allow the LLM to ask clarifying questions