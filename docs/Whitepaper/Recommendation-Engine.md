# Recommendation Engine
## Introduction
Recommendation enginds serve several purposes for a content distribution service. Given a system of videos, users, producers, and advertisers, a recommendation engine can help efficiently:

- Recommending Videos to watch
- Recommending Ads for specific users watching specific videos
- Recommending prices dynamically for users and content

We outline two approaches to this. In one, Paratii maintains a centralized recommendation engine for these purposes.
## Centralized Recommendation Engine
### Purpose

The goal here is to build a system that recommends videos to users based on their previous watching histories. The information used for this engine may not include the actual contents of the video since building an entire video processing unit within the Paratii system should not be necessary given historical user data involving tags and likes.

In practice, this means recommended playlists and/or videos that follow each video watched, and one or more playlists of recommended videos for the user at his "playlists" homepage.

### Categorizing Users and Videos with K-modes

In this section, we will apply the K-modes algorithm to classify both users and the videos they watch.

#### Introduction to K-modes

In the K-modes algorithm, one creates a series of K categories from the labels applied to data. These labels could be thought of as potential questions we could ask about the data to be classified. Like "Is it green?" or "Did she like that video with the cat?" The K-modes algorithm strives to determine K categories with the most common answers to these questions within each category and the most dissimilarity between categories.

[Python implementation here](https://github.com/nicodv/kmodes/tree/master/kmodes)


Each piece of data stores one or more "votes" for each label. For instance, a user would contain a series of labels that correspond to the question "Did you like video X?" and "Did you dislike video X?". A video would have labels of the form "Could this video be tagged as 'T'?", and this label could receive one or more votes by its viewers. For instance, two users could tag the video "cat", and as a piece of data, the video would carry two counts for the label "cat".

#### Categories

Each label in a category stores the sum of all the counts of each piece of data that currently belongs in that category. For instance, a category for video tags might be the tags "funny" and "cat". Then this category's "funny" label stores the sum of all the data placed in that category that contains the label "funny" and the same for "cat". As another example, a category of users would contain a tally of likes for each video rated by users within that category.

#### Dissimilarity

The K-modes algorithm tries to minimize the dissimilarity between elements in the same category. To do this, we need some notion of dissimilarity. This tends to be application specific, but for the purposes of this text, let's talk about two in particular:

- **Dissimilar Users**: Users are represented by their likes and dislikes. Dissimilar users disagree on whether they like or dislike a video. So the dissimilarity metric is the total number of videos the two users in question have rated differently divided by the total number of videos the two users have both rated.

- **Disimilar Videos**: Videos are a represented by a tally of tags. The dissimilarity between two videos can be the number of tallies in tags that the two videos don't have in common divided by the total number of tallies for all tags applied to both videos.

When a new label is applied, we compare all the labels applied to the newly labeled data with the labels used to describe each potential category. If the data still has the least dissimilarity with its current category, nothing changes. However, if it now is best described by a different category, we update the definitions of the category it left and the category it was transferred to so that each of those categories now reflects the lost and gained tallies of the data that was transferred.

#### Applications

We now apply K-modes to the two most important recommendation problems for Paratii:

Recommending a playlist and/or videos similar to the one just watched, and recommending a playlist for a user's "playlists page" with videos that the user should find appealing based on their history.

### Video Playlists

We first refer to the category the video has been placed in due to K-modes clustering of its tags, and choose videos to play next from that category. Since we already have a notion of dissimilarity, we can create a list of N videos with the least dissimilarity to the one watched within the same category.

### User Playlists

We first apply K-modes to the user as described above, and select the videos with the highest number of likes minus the highest number of likes minus dislikes by within that cluster. We then knock off all the videos the user has already seen, and we have a neatly curated list of highly rated videos by similar users that the current one hasn't seen. The list can be continuously updated as the user watches videos suggested by it.

#### Introduction to Neural Networks

Neural networks currently drive a wide variety of predictive frameworks. They have been used to translate texts, identify pictures, and even write music. In the most general case, they are given labels about something and return one or more new labels. 

For the purposes of this section, we outline how to use a shallow neural network to recommend videos to users given the user's likes and dislikes, and who has liked and disliked different videos. It's worth noting that there are many ways to creatively design complex network architectures that solve this problem. In this section we want to explore but the simplest design.

#### Neurons and Weighted Voting

In machine learning, a neuron is something that takes a label (an answer to any question) and returns a number. The label is usually first represented by a number or a set of numbers. Like a "yes" or "no" answer to a question might be represented by a "1" or a "0" respectively. A question like is it "red, green or blue" might have an answeras a series of numbers, {1, 0, 0} for "red", {0, 1, 0} for "blue" and {0, 0, 1} for "green" (called "one-hot vectors). You might also have a label for "how hot is it?" that is a number that takes a range.

Neurons track a few internal parameters internally that are used to turn input labels into output labels. As the parameters are changed, the neuron will produce different output labels from the same set of inputs labels. During the "training" phase, output labels can be compared against true labels, and the internal paramters are changed so the neuron is more likely to produce the true output labels given those same input labels.

#### Combining Neurons into a Network

Neural networks can be composed of many neurons strung together such that one or more outputs of one neuron feed into the inputs of other neurons. It turns out feeding the output of one neuron into the input of another can be loosely thought of as creating a "higher order" thought process which greatly increases the potential complexity in the network's decision making capability. If one neuron is used to label something "red" "green" or "blue", the next one might combine this neuron's output with many others to decide if we are looking at a dog, a street sign, or a firetruck. Okay, this is oversimplified a bit, but I think it provides good intuition for analyzing neural network structures. We will stick with a shallow network without any layering of neurons.

#### Using Single Sigmoid Layer To Recommend Videos

A very simple neural network achitecture consists of one neuron that votes for different labels based on a series of inputs. It does so with what is called a sigmoid function. The input labels answer the question "Did user X like video Y?" and "Did user X dislike video Y?" for all users X and vidoes Y in the system. The output label answers the question "Would this user like video Y?" for all videos Y in the system.

In our example, an answer of "yes" is represented by a "1" and "no" is "0". As an output, each user is assigned a sigmoid function that recommends videos. This sigmoid function returns a number between 0 and 1 for each video in the Paratii system that could be recommended to the user. The sigmoid function contains two internal parameters used to connect each input label to each video that the network might recommend.

Each user is assigned one sigmoid layer. The user's sigmoid layer consists of one sigmoid unit per video on the Paratii system. Each sigmoid unit contains two paramters per like and dislike recorded on the Paratii system. For ease of computation, this list can be truncated to N randomly chosen ratings. New sigmoid units can be added as new videos are added to the system. 

Each sigmoid unit computes a weighted sum of the likes and dislikes of all other users (each represented as a 1 or a 0) then sends the result through a [sigmoid function](https://en.wikipedia.org/wiki/Sigmoid_function). This results in a number between 1 and 0. An output closer to 1 represents a higher confidence of a user liking the corresponding video. The top k outputs are recommended on the user's playlist. When a user watches a video, the corresponding sigmoid unit is dropped.

When a user actually chooses a video, the weights are updated using [backpropagation](https://en.wikipedia.org/wiki/Backpropagation) to make better recommendations.

Note that this can be done completely offline. When a user connects to the player, the current weights are loaded into the player and updated as the video makes choices. The wieghts should be synced to the blockchain or a third party database periodically so the user can pickup where they left off the next time they connect to the Paratii player.

#### Using GRUs to Recommend the Next Video

We can modify the above sigmoid approach by using recurrent neural networks to recommend the next video a user should watch given what they just watched. The system would operate almost identically to the sigmoid version except the sigmoids are replaced with [GRUs](https://en.wikipedia.org/wiki/Gated_recurrent_unit). GRUs have an extra set of internal paramters that are updated continuously as the user chooses videos to watch out of the top K choices presented. A GRU can be thought of as a function that locks in on how to  *transition* between videos rather than the best video itself.

What's interesting about recurrent units like GRUs is that they contain information derived from their entire history of recieving inputs. Like sigmoids and all neurons, they have weights which are adjusted so they produce the best possible predictions. Recurrent units however also have "state" vectors that are changed between inputs. 

A user's history of clicking recommendations uniquely represents them as an individual within the recommendation engine through their state vectors. Since the GRU's weights encode transitions given state vectors, users can share weights and only be given their own state vectors that change as they use the system. The weights become the fruit of Paratii gathering information about user behaviors and each user's state vectors encode how the user interacts with the weights to recommend the best video. 
In the case that Paratii sponsors an in-house recommendation engine, some simple 

## Decentralized Recommendation Engine

### Introduction
We hope to outline a system that allows the community to build their own recommendation engines at first for Paratii, but ideally for arbitrary tasks as well. We outline two approaches. In the first we stick with neural networks and allow users to both collaborate and compete to contstruct arbitrary neural networks. In the second, users can treat each other as black boxes, and we make no assumption into the underlying nature of how they make predicions. 

### Agents and Data Feeds
The best recommendation engines continuously improve and adapt using feedback from the real world. As summarized in the [OpenAI](https://gym.openai.com/docs/) docs, a recommender, or *agent*, can interact with a subscriber, or *environment*, and receive feedback in the form of an *observation* and a *reward*. Agents adapt to maximize their rewards given their observations. Subscribers may ask multiple agents for recommendations, agents may adapt from feedback from multiple subscribers, and agents may provide multiple recommendations with associated "confidence levels". 

Agents are very general entities here, and may be designed to poll other agents to craft better recommendations. In this case, agents can act as subscribers as well and can work symbiotically within the same ecosystem. This is in contrast to multiple agents being polled by the same subscriber, in which case they compete to produce the best recommendations.

#### Application: Recommending Videos
A real life application within the Paratii ecosystem could include a recommendation for what video to watch next. The agent is given the video the user just watched, and must provide the user with a set of videos that the user might want see next. In response, the agent is provided with an *observation* regarding what the user actually chose to watch next, or whether they disconnected and did not watch any of the suggestions. The subscriber provides a *reward* in tokens if the user watches one of the recommended videos.

##### Subscribing to Other Agents
It may be adventageous for agents to build on recommendations from other agents. For example, one agent may classify a video according to some set of categories, another agent may classify a user according to some set of categories, and a third agent may use those two categorizations to match users to videos. 

#### Application: Recommending Ads
As a more complex example, imagine a content provider has subscriptions to several ad producers. The content provider allows users to stream videos and occasionally inserts ads from the ad providers. Each ad producer gives the content provider some reward when one of their viewers clicks on their ad. The content provider sends queries to an agent to recommend which ads to play when different users watch different videos. 

In this case, an agent receives a query from the content provider that contains the following information:

- The user
- The video the user is watching
- Rewards in the form of cuts from the ad revenue for the user watching each ad

The agent must recommend the ad that maximizes their cut of the ad revenue given its best estimates for which ads the user would click on the first place. 

##### Recommending Times for Ad Interruptions
A more complex variant could involve recommending the best time to interrupt the video with the ad. Participating agents would have to experiment on videos to determine which points can best be used as "cliff hangers" to place an ad. Alternativey, a content provider might query separate agents that just try to figure out the best time to place ads in different videos. They'd perhaps just be given the video and would return one or more times to insert ads associated with different conficence levels.

### Prediction API
Subscribers and agents need to be able to interact through a basic API that they can use to agree upon the format of the input data, the format of the recommendation data, and the format of the observation. Recommenders would publish their services on web3 homepages that document their contract's API. The API must include the following information:

- Format of the input data
- Format of observations

### Communication Via Smart contract
Since agent-environment systems are versatile and well studied, it should be sufficient to enforce communication between agents and subscribers through a common smart contract. Agents would register the format of their input and output data, and a smart contract address to call for predictions. Subscribers would call the contracts and provide rewards and observations. It would be entirely up to the agent whether or not to provide predictions, and up to the subscriber to provide a reward. This way bad agents can be ignored by subscribers and subscribers that always have a low probability of sending a reward (maybe they're just not paying) can be ignored by agents. 
