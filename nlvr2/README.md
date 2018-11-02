# Cornell Natural Language for Visual Reasoning for Real (NLVR2)

Website: http://lic.nlp.cornell.edu/nlvr/

The corpus and task are described in: A corpus for reasoning about natural language grounded in photographs. Alane Suhr, Stephanie Zhou, Iris Zhang, Huajun Bai, and Yoav Artzi. ArXiv preprint, https://arxiv.org/abs/1811.00491.

## Repository structure
The `data` directory contains JSON files representing the training, development, and public test sets. The `util` directory contains scripts for downloading the images, as well as hashes for all images.

## JSON files
Each line includes one example, represented as a JSON object. The critical fields are:

* `sentence`: The natural language sentence describing the pair of images for this example.
* `left_url`: The URL of the left image in the pair.
* `right_url`: The URL of the right image in the pair.
* `label`: The label: true or false.
* `identifier`: The unique identifier for the image, in the format: `split-set_id-pair_id-sentence-id`. `split` is the split of the data (train, test, or development). `set_id` is the unique identifier of the original eight-image set used in the sentence-writing task. `pair_id` indicates which of the pairs in the set it corresponds to (and is between 0 and 3). `sentence-id` indicates which of the sentences is associated with this pair (and is either 0 or 1 -- each image pair is associated with at most two sentences).

Some other useful fields are:
* `writer`: The (anonymized) identifier of the worker who wrote the sentence. The identifiers are the same across splits of the data.
* `validation`: The initial validation judgment, including the anonymized worker ID and their judgment.
* `extra_validations`: In the development and test sets, this is the set of extra judgments acquired for each example, including the anonymized worker ID and their judgment.
* `synset`: The synset associated with the example.
* `query`: The query used to find the set of images. You can ignore the numbers suffixing the query; these uniquely identify image sets for each query. 

`test1.json` includes the public test set.

We assume a consistent naming of the image files associated with each example. Given the identifier `split-set_id-pair_id-sentence-id`, the left and right images are named `split-set_id-pair_id-img0.png` and `split-set_id-pair_id-img1.png` respectively. Despite the extension of `.png`, not all images are actually PNGs. However, most image displaying software as well as libraries like PIL will process images according to the headers of the files themselves, rather than the extension. We only ran into problems using the default file browser in Ubuntu, and instead used imagemagick to browse images on that platform.  

## Downloading the images
The script `util/download_images.py` will download images from the URLs included in the JSON files. It takes three arguments: the path of the JSON file, a directory to which the images will be saved, and the path of the hash file (hash files are included in `util/hashes/`). For each image, the script sends a request to the URL, as long as the image is not downloaded yet, and compares it with the saved hash. We use the `imagehash` library for comparing the hashes. There is a timeout of two seconds for downloading. In addition, the script should catch any errors with accessing the URL. 

If the image could not be downloaded, it saves the identifier to a file (`*_failed_imgs.txt`). If the hash was not expected, it saves the identifier to a different file (`*_failed_hashes.txt`). For each image attempted, it saves it to a file (`*_checked_imgs.txt`). This allows you to stop and restart the download without going over images that have already been checked.

In total, the download can take a long time. This script took about a day to run on the development set. In addition, because the data was collected over the course of a year, some URLs are no longer accessible. We estimate that about 5% of the data is inaccessible from the saved URLs.

## Direct image download
We do not own copyright for the images included in the dataset. Thus, we cannot share the images publicly. However, we can provide direct access to the images as long as you are using them for research purposes. To obtain access, please email `NLVR2 EMAIL HERE`. We will ask you to agree to a terms of service before providing the images to you.

## Evaluation scripts

## Running on the leaderboard held-out test set

## Note about sampling a validation set