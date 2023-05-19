# Applying Lesson 1
After completing lesson 1, which focused on classifying whether an image was a bird or not, I adapted the provided _00-is-it-a-bird-creating-a-model-from-your-own-data_ code to now classify 10 different types of animal. To do this, instead of downloading bird and forest images, photos of 10 chosen animals were searched for. 

When trying to resize the downloaded images, I experienced a warning that I did not encounter with the original code. This error was _UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images_. After briefly looking into the warning, it appeared to be an issue associated with PNG images, so my solution was to add code to remove all images of this format:
```
searches = 'dog','cat','lion','turtle','flamingo','giraffe','panda','whale','goat','chicken'
path = Path('animals')

for o in searches:
    for file in os.listdir(path/o): 
        if file.endswith('.png'):
             os.remove(path/o/file)
    resize_images(path/o, max_size=400, dest=path/o)
```

As most photos were not PNG images, only a small percentage of the images were deleted. To compensate for this, I adapted my original search to now download a maximum of 250 images (instead of the original 200). This method succesfully solved the problem, however there appears to be other solutions that don't require all PNG images to be deleted [^1]. This number of max images to be downloaded was also found to give the best performance with increasing further resulting in unrelated/unwanted images being downloaded.

Something I noticed was that the code provided in lesson 1 of the fastai code made three different searches for animal photos including sun and shade photos. For example for giraffe photos, three different search terms are used: 
- giraffe photo
- giraffe sun photo
- giraffe shade photo

I assume this was done to train the model with images of varying lighting, however it gave rise to some interesting results. For example, the searches which included the _shade_ term often resulted in images with the animal on a lamp shade as below:
![giraffe lampshade](https://cdn.notonthehighstreet.com/fs/15/ef/b5bb-3c1c-430a-a39e-e281ddc90e0e/original_giraffes-lampshade.jpg)

The searches including the _sun_ term also resulted in unwanted images such as a lion sun tattoo: 
![lion sun tattoo](https://s-media-cache-ak0.pinimg.com/736x/91/6e/ff/916effac525e26e7420f54790722800f.jpg)

From this discovery, I ended up deleting the searches for sun and shade images and improved classification was achieved:
| Condition | Incorrect Classifications |
|-|-|
| Sun and Shade Photos Included | 11% |
| Sun and Shade Photos Excluded | 1.8% |

[^1]: [relevantThread](https://stackoverflow.com/questions/70839890/pil-remove-error-userwarning-palette-images-with-transparency-expressed-in-byt)
