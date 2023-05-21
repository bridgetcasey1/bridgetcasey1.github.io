# Applying Lesson 1
After completing lesson 1, which focused on classifying whether an image was a bird or not, I adapted the provided _00-is-it-a-bird-creating-a-model-from-your-own-data_ code to now classify 10 different animals. To do this, instead of downloading bird and forest images, photos of the 10 chosen animals were searched for. 

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

As most of the downloaded photos were not PNG images, only a small percentage of the images were deleted. To compensate for this, I adapted my original search to now download a maximum of 250 images (instead of the original 200). This method succesfully solved the problem, however there appears to be other solutions that don't require all PNG images to be deleted [^1].

Something I noticed was that the provided _is-it-a-bird_ code made three different searches, which included searching for sun and shade photos. For example, when searching for bird photos, three different search terms are used: 
- bird photo
- bird sun photo
- bird shade photo

I assume this is done to train the model using images of varying lighting. However, it gave rise to some interesting results when I adapted the code to search for the 10 animals. For example, the searches which included the _shade_ term often resulted in images with the animal on a lamp shade as below:
![giraffe lampshade](https://github.com/bridgetcasey1/bridgetcasey1.github.io/assets/113487655/d92339bd-a881-4559-9b17-5546ce25611c)

The searches including the _sun_ term also resulted in unwanted images such as a lion sun tattoo: 
![lion sun tattoo](https://github.com/bridgetcasey1/bridgetcasey1.github.io/assets/113487655/8c11a78e-3658-4441-b596-57c360baab4d)

From this discovery, I ended up deleting the searches for sun and shade images and searching for animal photos only and improved classification was achieved:

| Condition | Classification Accuracy |
|-|-|
| Sun and Shade Photos Included | 89% |
|-|-|
| Sun and Shade Photos Excluded | 98.2% |

If interested in the code I have included my notebook here: [Animal Classification Notebook](/pdf/ELEC4630_A3_Q2.pdf).

[^1]: [Relevant thread](https://stackoverflow.com/questions/70839890/pil-remove-error-userwarning-palette-images-with-transparency-expressed-in-byt)
