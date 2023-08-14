**MODEL**
Four ConvNeXt-V2 Base models are used in this submission. The models each predict one score: extent_left, extent_right, density_left, or density_right. The individual scores are then combined to obtain the mRALE score.

**DATA**
All models were initialized with ImageNet-22K pretrained checkpoints, and were then trained on the mRALE data. No other data was used in this submission.
