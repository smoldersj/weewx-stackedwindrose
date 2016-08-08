# Stacked Windrose Image Generator for weewx

## Description
The Stacked Windrose Image Generator for weewx is a weewx extension that generates a polar windrose plot image file based upon weewx archive data. The image file is suitable for publishing on a web page, inclusing in a weewx template or for use elsewhere.

The extension consists of a single weewx skin that controls a custom image generator. Various image parameters including the plot period, source data field, units of measure and colours can be controlled through settings in weewx.conf.


## Pre-Requisites

Weewx v3.2.0 or greater.


## File Locations

As Weewx file locations vary by system and installation method, the following 
symbolic names, as per the Weewx User's Guide - Installing Weewx, are used in 
these instructions: 

- $BIN_ROOT (Executables)  
- $SKIN_ROOT (Skins and templates)  
- $HTML_ROOT (Web pages and images)  

Where applicable the nominal location for your system and installation type 
should be used in place of the symbolic name


## Installation Instructions

1.	Download the Stacked Windrose Image Generator for weewx extension from the 
Stacked Windrose Image Generator for weewx GitHub repositry.

1.	Run the installer

    	$ ./wee_extension install=stackedwindrose-X.Y.Z.tar.gz
    
    This command assumes the user is currently in the $BIN_ROOT folder.

1.	By default the extension uses metric units (km/h) and 'd/m/y' date formats. If you wish the generated plot(s) to use DIFFERENT units or date formats, then the following settings should be edited under the `[[StackedWindRose]]` sub-section in the `[StdReport]` section of `weewx.conf` and set as required:

	    [Units]
	
	        [[Groups]]

	            # group_speed sets the display units for the windrose plots
	            group_speed = km_per_hour   # Options are 'mile_per_hour', 
	                                        # 'km_per_hour', 'knot', or 
	                                        # 'meter_per_second'
	        
	    [ImageStackedWindRoseGenerator]
	
	        [[day]]
	            [[[daywindrose]]]
	                [[[[windGust]]]]
	                    time_stamp = %H:%M %d %b %Y

	**Note**: Some settings were omitted from the above extract for brevity.

1.	By default the generated file(s) are placed in the $HTML_ROOT folder. If you wish to place the generated files in another directory then insert a `HTML_ROOT` setting under the `[[StackedWindRose]]` sub-section in the `[StdReport]` section of `weewx.conf` eg:

		[[StackedWindRose]]
        	skin = StackedWindRose
			HTML_ROOT = /use/this/folder

1.	Restart Weewx:

    	$ sudo /etc/init.d/weewx stop
	    $ sudo /etc/init.d/weewx start

1.	This will result in the windrose image file being generated during each report generation cycle.


## Manual Installation Instructions

1.	Download the Stacked Windrose Image Generator for weewx extension from the 
Stacked Windrose Image Generator for weewx GitHub repositry.

1.	If necessary extract the files from the compressed archive file and copy files as follows:

    	$ cp bin/user/*.py $BIN_ROOT/user
    	$ cp -R hdfw/skins/* $SKIN_ROOT

1.	In `weewx.conf`, modify the `[StdReport]` section by adding the following sub-section:

    	[[StackedWindRose]]
        	skin = StackedWindRose

1.	Add the following 

	        
	    [Labels]
	
	        [[Generic]]
	            
				# This section sets the labels to be used when displaying 
				# either windGust or windSpeed
	            windGust  = Gust Speed
	            windSpeed = Wind Speed
	            
	    [ImageStackedWindRoseGenerator]
	
	        # This section contains the options for the Weewx-WD Stacked Wind Rose image generator.
	        # A number of plotting options can be set such as colour, fonts, sizes etc. Some options
	        # have default values if the option or value is omitted, these defaults are specified
	        # where applicable.
	        #
	        # Fonts can be anything accepted by the Python Imaging Library (PIL), which
	        # is currently truetype (.ttf), or PIL's own font format (.pil). See
	        # http://www.pythonware.com/library/pil/handbook/imagefont.htm for more
	        # details.  Note that "font size" is only used with truetype (.ttf)
	        # fonts. For others, font size is determined by the bit-mapped size, usually
	        # encoded in the file name (e.g., courB010.pil).
	        #
	        # Colors can be specified any of three ways:
	        #   1. Notation 0xBBGGRR;
	        #   2. Notation #RRGGBB; or
	        #   3. Using an English name, such as 'yellow', or 'blue'.
	        # So, 0xff0000, #0000ff, or 'blue' would all specify a pure blue colour.
	
	        # Image file to be used to used as the background for the plot.  If it does not exist
	        # or is set to none/None the background will be image_height x image_width pixels with
	        # colour set to image_background_box_color.
	        # Example setting:
	        # image_background_image = windrose_background.png
	        image_background_image = none
	
	        # Overall width and height of image
	        image_width = 382
	        image_height = 361
	        # Fill colour of plot circles
	        image_background_circle_color = 0xF5F5F5
	        # Colour of background box
	        image_background_box_color = 0xF5C696
	        # Colour of range rings on plot
	        image_background_range_ring_color = 0xC3D9DD
	
	        # Minimum distance (in pixels) from edge of image to plot/legend
	        windrose_plot_border = 5
	        # Width of legend stacked bar (in pixels)
	        windrose_legend_bar_width = 10
	
	        # Font to be used
	        windrose_font_path = /usr/share/fonts/truetype/freefont/FreeSansBold.ttf
	        # Font setting for N,E,S,W, bullseye % and % rings
	        windrose_plot_font_size  = 10
	        windrose_plot_font_color = 0x000000
	        # Font setting for plot legend
	        windrose_legend_font_size = 10
	        windrose_legend_font_color = 0x000000
	        # Font setting for plot label/title
	        windrose_label_font_size = 12
	        windrose_label_font_color = 0x000000
	        # Comma separated list of colours used for stacked 'petals' on wind rose.
	        # First colour is for Calm/0 speed, 2nd for 1st band, 2nd for 3rd band etc.
	        # Defaults to 'lightblue','blue','midnightblue','forestgreen','limegreen','green','greenyellow'
	        # Only 7 colours will be used.
	        windrose_plot_petal_colors = aqua, 0xFF9900, 0xFF3300, 0x009900, 0x00CC00, 0x33FF33, 0x00FFCC
	        # Width of petals in degrees. If not set default is 16 degrees
	        windrose_plot_petal_width = 16
	
	        [[day]]
	            # Period (in seconds) over which wind rose is constructed. 86400 will use wind
	            # samples from past 24 hours, 43200 uses 12 hours of samples etc
	            period = 86400    # == 24 hours
	
	            [[[daywindrose]]]
	                # Image filename will be [[[]]] setting on line above with extension as
	                # per format setting below. [[[daywindrose]]] using png format will result
	                # in file being named daywindrose.png
	
	                # File format of resulting image file. Use any file extension for image
	                # formats that PIL can write (eg png, gif etc). Defaults to png if omitted
	                format = png
	
	                # To use windGust data for wind rose set [[[[windGust]]]] on next line,
	                # to use windSpeed set next line to [[[[windSpeed]]]]
	                [[[[windGust]]]]
	                    # Label/title for  plot
	                    label = 24 Hour Wind Rose
	                    # Format of optional time stamp to be placed on image. Format codes
	                    # as per Python datetime format directives at
	                    # https://docs.python.org/2/library/datetime.html
	
	                    #time_stamp = %H:%M %d %b %Y
	
	                    # Location of optional time stamp on image. If omitted no time stamp is
	                    # included. Format for setting is vertical_position, horizontal_position
	                    # where vertical_position is one of top or bottom; horizontal_position is one
	                    # of left, center, right. Default setting (if option is present) is
	                    # bottom, right
	
	                    #time_stamp_location = bottom, right

1.	By default the extension uses metric units (km/h) and 'd/m/y' date formats. If you wish the generated plot(s) to use DIFFERENT units or date formats, then the following settings should be edited under the `[[StackedWindRose]]` sub-section in the `[StdReport]` section of `weewx.conf` and set as required:

	    [Units]
	
	        [[Groups]]

	            # group_speed sets the display units for the windrose plots
	            group_speed = km_per_hour   # Options are 'mile_per_hour', 
	                                        # 'km_per_hour', 'knot', or 
	                                        # 'meter_per_second'
	        
	    [ImageStackedWindRoseGenerator]
	
	        [[day]]
	            [[[daywindrose]]]
	                [[[[windGust]]]]
	                    time_stamp = %H:%M %d %b %Y

	**Note**: Some settings were omitted from the above extract for brevity.

1.	By default the generated file(s) are placed in the $HTML_ROOT folder. If you wish to place the generated files in another directory then insert a `HTML_ROOT` setting under the `[[StackedWindRose]]` sub-section in the `[StdReport]` section of `weewx.conf` eg:

		[[StackedWindRose]]
        	skin = StackedWindRose
			HTML_ROOT = /use/this/folder

1.	Restart Weewx:

    	$ sudo /etc/init.d/weewx stop
	    $ sudo /etc/init.d/weewx start

1.	This will result in the windrose image file being generated during each report generation cycle.