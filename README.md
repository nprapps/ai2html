# ai2html for NPRAPPS

> ai2html is an open-source script for Adobe Illustrator that converts your Illustrator documents into html and css.

This is our fork of NYT's original [original repo](http://ai2html.org). See details on original contributors on their repo. 

For documentation and examples on [how to use ai2html](http://ai2html.org), please visit [ai2html.org](http://ai2html.org).

## NPR's changes to ai2html

### NPR's fonts

We removed NYTs fonts, and added our own. 

### Double text: a new feature for labeling

In html text it is difficult to add a high fidelity background stroke to text. This is often useful for text that sits over a busy background, as is common in cartography. NPR's has added a feature to ai2html called double text. 

When `double_text: true` is setting in your `ai2html_settings`, ai2html will create duplicate html text in the output for any text field that is stored inside a layer called `upper-text`. 

For example, if you had a label for the Himalayan Mountains in light gray over a busy terrain image. If this label lives in `upper-text`, the output after running ai2html might look like the following in html:

	<div id="g-ai0-2" class="g-upper-text g-aiAbs g-aiPointText" style="top:58.0685%;margin-top:-29.6px;left:66.0076%;margin-left:-101px;width:202px;">
		<p class="g-pstyle0">Himalayan</p>
		<p class="g-pstyle0">Mountains</p>
	</div>
	<div id="g-ai0-2" class="g-lower-text g-aiAbs g-aiPointText" style="top:58.0685%;margin-top:-29.6px;left:66.0076%;margin-left:-101px;width:202px;">
		<p class="g-pstyle0">Himalayan</p>
		<p class="g-pstyle0">Mountains</p>
	</div>

Then, you need to apply the following css styles to account for the new `g-lower-text` content. 

	.g-lower-text {
	    -webkit-text-stroke-width: 4px; // some stroke amount
	    -webkit-text-stroke-color: rgb(169,145,127); // some color, per your pref.
	    z-index:0; // sit this layer beneath the upper text
	    stroke-linejoin: round;
	}

	.g-upper-text {
	    z-index: 1; // sit this layer above the lower text
	}

Note: there may be enhancements needed to make sure that screen readers ignore all `g-lower-text` text. 

### Other changes

Based on how NPR uses ai2html inside our systems, we made a few small tweaks to ensure files are output in the correct spot, with the correct namespace. These changes bring certain things back in line with how ai2html worked with v0.115. Your mileage may vary. 

#### Fixes to `project_name` field

The changes here are cataloged in a [pull request](https://github.com/newsdev/ai2html/pull/176) made to the NYT repo. I believe this fixes a bug that was introduced at some point in the last few versions, but it is possible that it is intentional.

#### Fixes (?) to `image_output_source` field

There's a (possible) bug in `image_output_source` field bug introduced ~v121. As of v121, the NYT version puts the image folder indicated in `image_output_source` relative to the location of the .ai file. With NPR's fix, the path of the image folder used in this field will be placed relative to the `html_ouput_path` field. This is consistent with v115.



