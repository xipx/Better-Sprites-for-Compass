Better-Sprites-for-Compass
==========================

To remember those mixins and functions Compass provides us is a quite tedious labor.
Using these mixins to make sprites a lot easier.

##How to use
###Remove the random string of sprite filename
Add the following code to your `config.rb`. This little piece of code can remove the random string appended to sprite filename.
```ruby
on_sprite_saved do |filename|
  if File.exists?(filename)
    FileUtils.cp filename, filename.gsub(%r{-s[a-z0-9]{10}\.png$}, '.png')
  end
end

on_stylesheet_saved do |filename|
  if File.exists?(filename)
    css = File.read filename
    File.open(filename, 'w+') do |f|
      f << css.gsub(%r{-s[a-z0-9]{10}\.png}, '.png')
    end
  end
end
```

###1. Include **Compass** and the **mixin file**.

```scss
@import "compass";
@import "sprite-background";
```

###2. Initialization by using the `get_sprites_obj()` function.

Before using the mixin, you need perform some initialization by using the `get_map_obj()` function.

The `get_sprites_obj()` function takes one or more **sprite-folder** for sprite-genaration purpose.
And you have to assign the returned value to a variable named `$sprites_obj`, because this variable is used by the mixin later on.

For example, suppose your project's `images` folder structure like this

```
.
└── images
    ├── foo
    │   ├── bar.png
    │   └── foobar.png
    └── foo_retina
        ├── bar.png
        └── foobar.png
```

Then load your sprite-folders like this
```scss
$sprites_obj: get_sprites_obj( foo, foo_retina );
```

where image-files in the `foo` folder are those regular version, and in the `foo_retina` folder are retina version.

Note that, the `_retina` suffix is **required** and the filename should be **same**.


###3.Use the mixin
`bar.png` in `foo` folder :
```scss
@include _sprite-background(bar, foo);
```

Enable retina version :
```scss
@include _sprite-background(bar, foo, $retina:true);
```

Sometimes you really don't want the `width` and `height` :
```scss
@include _sprite-background(bar, foo, $dimension:false);
```

If you need, you can adjust the offsets of `background-position` by doing this :
```scss
@include _sprite-background(bar, foo, $offset-left:10px, $offset-top:5px);
```

