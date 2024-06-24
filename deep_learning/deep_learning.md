
# Problems
## Jumping loss at the beginning of each epoch 
When I trained an MLP with supervised learning, I encountered a strange problem. At the start of each epoch, the loss initially spikes before gradually declining. It turns out that `Numpy`'s shuffle's randomness is not enough. Using shuffle of `Pytorch`'s `dataloader` solves this problem.

