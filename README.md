<h1 align="center">SAM PROJECT</h1>
This project creates one segmented image using "SAM" (segemnt anything model by facebook ) from a video.

## info
  When we create a video even if it seem to our eyes a constant video there will be some changes in the video that the computer will recognize;
  SAM model is a super powerful model that can take any kind of photo and create a segement image from it.
  The project i made takes all the frames from a disired video and segemnt them, then combine all the segmented images into one that will be the summary of all images.
  SAM gives as a list of masks about every image we pass to the program,
  - Each mask constain
    - `segmentation` - `[np.ndarray]` - the mask with `(W, H)` shape, and `bool` type
    - `area` - `[int]` - the area of the mask in pixels
    - `bbox` - `[List[int]]` - the boundary box of the mask in `xywh` format
    - `predicted_iou` - `[float]` - the model's own prediction for the quality of the mask
    - `point_coords` - `[List[List[float]]]` - the sampled input point that generated this mask
    - `stability_score` - `[float]` - an additional measure of mask quality
    - `crop_box` - `List[int]` - the crop of the image used to generate this mask in `xywh` format
## Use similar masks and weights
    So firstly we should put in a list all the similar masks between all the image segmentations according to the values 
    of 'area', 'bbox','point_coords' (allow 5% diffrent between "similar" masks), and now we should iterate over every group
    of similar masks and create one combined mask from it.
    The key is to use the 'predicted_iou' which it is the prediction for the quality of the mask; so we should take
    the relative part of each mask from the group and use weights for every mask.
    Then we should combine all similar masks into one using 'weights';
 ## How to really combine masks
        We need to create a combination of multiple `[np.ndarray]`of `bool` type.
        So i changed the type of the np.ndarray to int's (1/0 - true/false) and not boolean, than a multiple any np.ndarray
        of each mask according to the matching cell in `weights` and created combined np.ndarray of int's, to return it back to
        be `bool` type if the cell equal or greater then 0.5 the cell will be true else it's false.
        So now we have combined masks of all segmentations.
