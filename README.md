# alpha-restore-test

Testing a trivial example of alpha decompositing (restoring alpha channel from a known mask with a known background)

Flattening is done by `imagemagick`, e.g. for black, RGB = `(0,0,0)`:

```sh
convert test-squares_20-40-60-80.png -background "rgb(0,0,0)" -flatten flat.png
```

From left to right then top to bottom):

## Before

- 20% (140, 140, 140)
- 40% (120, 130, 210)
- 60% (200, 100,  80)
- 80% (255, 240, 130)

## After

- (28,   28,  28)
- (48,   52,  84)
- (120,  60,  48)
- (204, 192, 104)

## Equation

When black is the background colour, the alpha compositing is linear:

```
(0.2R, 0.2G, 0.2B) = (28,   28,  28)
(0.4R, 0.4G, 0.4B) = (48,   52,  84)
(0.6R, 0.6G, 0.6B) = (120,  60,  48)
(0.8R, 0.8G, 0.8B) = (204, 192, 104)
```

## Limits

What are the limits of visibility for semitransparent pixels then?

For example, if a pixel is `(4,4,4)` and flattened onto `(0,0,0)`
then what is the upper limit on its restorable transparency?

If the pixel has 80% transparency, i.e. alpha 0.2 then the restoration equation is:

```
(0.2R, 0.2G, 0.2B) = (0.8, 0.8, 0.8)
```

...but if all values are being coerced to integers then this will in fact round up
to `(1,1,1)` and remain visible against a black background!

So in fact the limit on transparency is what brings it under `0.5`, not under `1`,
i.e. half the alpha limit you'd expect: `12.5%` or ⅛.
