# SpaceTradersAI

## Introduction
An AI to play the SpaceTraders.io game

SpaceTraders is an open-universe, programmable fleet-management game played through a headless API. Thus, it can be entirely played by making GET, POST, PATCH calls to the endpoint.
You can trade goods across systems, mine asteroids for valuable ores, or take on faction contracts to earn credits and reputation.

## Documentation
You can learn more about the game here - https://spacetraders.io/
The documentation for the API is availabel here - https://docs.spacetraders.io/. The documentation was generated using OpenAPI.

## Goal
The goal of this work is to build an AI that can play the game in an automated fashion, once the user specifies a set of goals they want to achieve.

## Work in progress
The SpaceTraders-AI jupyter notebook currently does the following - 
1. 

## Limitations
The primary limitations are - 
1. The context length is greater than 4000 tokens but the combined prompt + completion length is limited to 4097 tokens. Hence, the LLM is unable to carry out complex tasks involving multiple steps and is limted to smaller tasks involving fewer API calls
2. The Chat model is not interactive so the entire documentation has to be fed in a single shot. 
3. There is no opportunity to answer any clarifying questions the agent might have. Hence, if any aspect of the goals are unclear, it fails.
4. Given the length of the documentation (over 50,000+ tokens), the LLM agent can lose context when trying to accomplish sophisticated goals involving multiple tasks. This can be minimized by making the chat interactive and priming the LLM with appropriate portions of the documentation when in doubt.

## Next steps
1. Make the chat model interactive by allowing it to ask clarifying questions