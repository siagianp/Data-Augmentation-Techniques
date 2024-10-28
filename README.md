from __future__ import division
import torch
import math
import sys
import random
try:
    import accimage
except ImportError:
    accimage = None
import numpy as np
import numbers
import types
import collections
import warnings

from PIL import Image, ImageFilter
from skimage import color
import cv2
from scipy.ndimage.interpolation import map_coordinates
from scipy.ndimage.filters import gaussian_filter
from torchvision.transforms import functional as F

if sys.version_info < (3, 3):
    Sequence = collections.Sequence
    Iterable = collections.Iterable
else:
    Sequence = collections.abc.Sequence
    Iterable = collections.abc.Iterable


# add annotation for class *RandomAffine*, how to use it. line 1040 -- 1046
# add new augmentation class *RandomGaussBlur* for gaussian blurring.
# add new augmentation class *RandomAffineCV2* for affine transformation by cv2, which can \
# set BORDER_REFLECT for the area outside the transform in the output image.
# add new augmentation class *RandomElastic* for elastic transformation by cv2
__all__ = ["Compose", "ToTensor", "ToPILImage", "Normalize", "Resize", "CenterCrop", "Pad",
           "Lambda", "RandomApply", "RandomChoice", "RandomOrder", "RandomCrop", "RandomHorizontalFlip",
           "RandomVerticalFlip", "RandomResizedCrop", "RandomSizedCrop", "FiveCrop", "TenCrop", "LinearTransformation",
           "ColorJitter", "RandomRotation", "RandomAffine", "Grayscale", "RandomGrayscale",
           "RandomPerspective", "RandomErasing",
           "HEDJitter", "AutoRandomRotation", "RandomGaussBlur", "RandomAffineCV2", "RandomElastic"]

_pil_interpolation_to_str = {
    Image.NEAREST: 'PIL.Image.NEAREST',
    Image.BILINEAR: 'PIL.Image.BILINEAR',
    Image.BICUBIC: 'PIL.Image.BICUBIC',
    Image.LANCZOS: 'PIL.Image.LANCZOS',
    Image.HAMMING: 'PIL.Image.HAMMING',
    Image.BOX: 'PIL.Image.BOX',
}
