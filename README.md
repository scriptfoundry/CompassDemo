CompassDemo
===========

A simple, step-by-step demonstration of some of the basic features of the Compass CSS framework. You can step through the demonstration by checking out each "tag" and studying the diff between each one to see the change. 

******

In this demonstration, we'll start with a simple web page and gradually transform the project into a complete Compass project, using sprites, variables, and functions. This is not a tutorial. And there's lot more I won't be going into, including mixins and extending classes -- for that, YouTube and Google is your friend. 

To go through the steps, use `git` to go through the different steps by doing a "checkout" of each commit in the series. Start at the very beginning, by checking out `step1`. Then go to the next step by checking out `step2`, and so on. You may want to delete any unstaged files that might have been left behind in the last step. You can do this by just selecting and deleting the files if you are using a Git GUI, such as SourceTree. If you are using the command line, you'll want to do something like this:

    git checkout step1    # checking out `step1`
    git clean --force -d  # cleaning out any files that aren't in the repo

## Requirements

To use Compass, you need to install the following:

 1. Ruby -- This is the core language behind Ruby on Rails. You can visit the main project page at [http://www.ruby-lang.org/en/downloads/](http://www.ruby-lang.org/en/downloads/), but to install Ruby on Windows, it is easier and much faster to go to [http://rubyinstaller.org/](http://rubyinstaller.org/). If you aren't sure if you have Ruby, try running these in the command line: `ruby -v` and `gem -v`.

 2. RVM -- This /may/ be necessary if you have ruby installed already and your version is incompatible with what Compass requires ()

 2. Compass -- For this, you need to run `gem update --system`, to make sure that the Ruby package management system (`gem`) is capable of installing Compass). Then, run `gem install compass` to install Compass. For more information, including information on creating new Compass projects, simply follow the instructions at the Compass project page, [http://compass-style.org/install/](http://compass-style.org/install/). 



## The steps

### 1. `step1` -- Unstyled content

This is a completely unstyled page with a handful of somewhat semantic HTML navigation elements. There is a file folder full of images, only one of which is actually used. There is no CSS and at this point, no Compass/Sass.

#### Try this:

At this point, just take a look at the files and get a sense where everything is. 

### 2. `step2` -- Styled "modern" CSS

This is styled page with hand-written CSS that makes the page look and behave as expected using "modern" HTML and CSS. This is how a page would probably be laid out without using sprites and without using Compass. The hover-state is controlled via CSS (where some people might argue it does not belong).

Note that because we're not preloading the images, they are loaded when the rollover happens. This will cause the images to "pop" into view after a delay. This won't be too bad on `localhost`, but on a slow or distant connect, the effect is brutal.

#### Try this:

At this stage, we're ready to try installing Compass. 

 1. Open a command prompt (in Windows, click on your `Start` button, select `Run` and enter `cmd`).
 2. Navigate to the project folder.
 3. Enter `compass init`.

This will create several files and folders, including:
 * `config.rb` -- Edit this file to control what Compass does when you run it
 * `/sass/` -- This is where you will store your Sass-style CSS files. Every `.scss` file that appears here will be compiled and stored in the folder below.
 * `/stylesheets/` -- You will never need to edit any of these files. But you will point your HTML files to use the files here. Each file you see here should have a corresponding `.scss` file in the `/sass/` folder.

#### Tips:

Instead of running `compass init`, you can also run the command `compass create <projectname>`. The only difference is that `compass init` will simply add the basic Compass files and folders to the folder you're in, while `compass create ...` will create a new folder containing the basic Compass scaffolding files. If you are starting a project from scratch, you may want to use `compass create`.

### 3. `step3` -- Compass first installation

This version includes the basic Compass files, including `config.rb` (a Ruby file) and three standard `scss` files (the actual Compass files). The project has not been "compiled" yet. You compile a project to create the CSS files (and other Compass-created assets) by running the command `compass compile`. Let's try doing that.

#### Try this:

First, let's clean out the compass project. You can do this either at the Git level (by running `git clean --force -d`). But let's try doing it with Compass now:

    compass clean 

or it's ever unsuccessful:

    compass clean --force

Now, let's compile:

    compass compile

Take a look at what happened. See what files were created. Try making a chance to one of the `.scss` files (such as `/sass/screen.scss`) and compile again. Maybe you got an error -- Compass will validate your Sass to make sure it's valid. Its validation rules are pretty strict, so simple CSS mistakes that might otherwise creep into your project will get stopped at this stage. That's a pretty nice feature. 

But you know, it's a huge hassle having to compile after every single change. Here, Compass has a utility that really helps. Try running this:

    compass watch

This will compile your project and then continuously check for changes to your `.scss` files. If you make a change, it will automatically compile the project again (or try to). 

Try making a change and see what happens to the HTML file in your browser! It can take a couple of seconds (or more) to compile the project, depending on how much it has to do, so your change might not show up immediately.

To stop watching, hit `Ctrl+c` (or `Command+c` on a Mac).

### 4. `step4` -- Building the sprite image

Making a sprite image is a huge pest. It requires pixel-precision and making changes can sometimes be a massive undertaking (especially if you need to rescale a bunch of stuff). Here's where the move to Compass really pays for itself. 

Here, we're only going to get Compass to build the sprite file. It can also build the CSS to support that sprite file, but we'll get to that in the next step.

#### Try this:

This is super-simple. You already have a folder full of your sprite images in `/images/textsprites/`. Note that the large background image is not part of this folder. What we'd like is to put all of those sprites into a single sprite image. To do this, we simply add this line to one of our `.scss` files:

    @import "../images/textsprites/*.png";

Now when you compile, it will take a lot longer to finish, and afterwards you should see at least a couple of changes. First, there will be a new image with a name like `images/textsprite-s12132a4b86.png`. There will also be a new entry in `css/screen.css` to include the image, but we won't worry about that just yet. For now, try adding a new image to your sprites folder and recompiling. Try removing some images and recompiling. Notice that the file name keeps changing every time something inside the sprites folder changes. 

### 5. `step5` -- Build the sprite image and add each sprite to the CSS

Just building the sprite image is nice -- it takes a load of work off the designer's list. But setting up all the rules is (sometimes) an even bigger task. So let's see how Compass helps.

#### Try this:

All we're going to do is add a couple more lines to the `screen.scss` file:

    @import "../images/textsprites/*.png";
	@include all-textsprites-sprites;
	$textsprites-sprite-dimensions: true;
	
And recompile. Unless you have make an image change, this should finish almost immediately. Take a look at `screen.css` and notice all the new sprite rules; for example:

    .textsprites-topnav_item8_on {
      background-position: 0 -130px;
      height: 24px;
      width: 80px;
    }

This means that if you apply the CSS class name, .textsprites-topnav_item8_on, to an element, it will instantly get a background that previously was in the image called `topnav_item8_on.png`. Note, that you may need to also apply `display:block`/`display:inlike-block` and `position:relative`/`position:absolute` to that element.

#### Now try this:

Sometimes, you don't need all the rules. Or sometimes, like with our rollovers, these rules aren't actually useful on their own. For this, we may want to check-pick the parts of the sprite we want to use. Let's clean up by reverting back to `step5` (we'll lose all our changes):

    git clean -f

Now, let's add these lines:

    @import "../images/textsprites/*.png";
    $textsprites-sprite-dimensions: true;

    .item2 {
        @include textsprites-sprite('topnav_item2_off');
    }
    .item2:hover {
        @include textsprites-sprite('topnav_item2_on'); 
    }

Recompile and see what it looks like. Well, the sprite is there, but so is the text that it's supposed to replace. So let's add three more lines to hide that text:

    .topnav { height:24px; }
    .topnav a, .sidenav b { display:inline-block; }
    .topnav b, .sidenav b {display:none;}

That works well. Now we just need to do the rest of the file. 

### 6. `step6` -- Add a nice linear gradient (using CSS3 with fallbacks)

CSS3 supports lots of really great advanced functions, but the browser coverage is spotty, and in some cases it is available only as an "experimental" feature that requires "browser prefixes". For many of these, Compass doesn't quite do all the work for you, but it makes it considerably easier. For example, let's look at replacing the grey linear gradient in the page background. 

#### Try this:

First, we need to import the functions that Compass needs. 

    @import "compass/css3";

Now, let's add both the fallback and the CSS3 rules. In general, include the CSS2 rules first, before adding the CSS3 rules.

    body {
      background: url(../images/background_gradient.png) repeat-x;
      @include background-image(linear-gradient(top, #818181, #ffffff 150px));
      background-repeat: repeat-x;
    }

Note that the `url()` is the fallback for browser that don't support CSS3 (IE and some mobile browsers).

### 7. `step7` -- Stop hard-coding "magic" numbers. Use variables instead.

In the last example, we used some "magic" numbers, `#818181` and `#ffffff`. It's not so bad right now, but imagine that these values appear in lots of places in your site design. Now imagine that you change the colour scheme a bit. How would you change it? Search-and-replace? Try using variables instead:

	/* The following five colors are assigned to variables */
    $color1: #9C4145;
    $color2: #C95D4C;
    $color3: #FF8C58;
    $color4: #FFDA96;
    $color5: #64876C;

Now we can use these colour variables

#### Tip:

Variables aren't just for numeric colour values. How about named colours? Or widths and sizes? How about font stacks?
    $color1: red;
    $columnwidth: 40em;
    $headlinefont: Papyrus, "MS Comic Sans";

#### Awesome tip:

Check this:

    $columnwidth: 50em;
    $columnimagepadding: 5px;
    $columncenter: $columnwidth / 2;
    $columnimagewidth: $columnwidth - ($columnpadding * 2);

### 8. `step8` -- Compass can help you do things

You can do some pretty neat things with Compass, via the extremely powerful functions that come with Sass.

Here's an example. 


### 9. `step9` -- The final project
Here, we've gone from `step2`'s 38 requests of 125KB, down to 3 requests totalling only 50KB. 

Because of the `:hover` in the navigation elements, the CSS isn't as simple as it might be, but the code is a lot more readable than before. There are fewer "magic" numbers, and it's a lot easier and safer to refactor than before.