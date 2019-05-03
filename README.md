# NAME

Geography::NationalGrid - Base class to create an object for a point and to transform coordinate systems

# SYNOPSIS

Geography::NationalGrid is a factory class whose sole purpose is to give you an object for the right country.
Geography::NationalGrid::GB and Geography::NationalGrid::IE are included with this distribution - other countries'
national grids are converted by other packages.

The first argument to new() is the ISO 2 letter country code, and it is followed by name-value pairs that are passed to 
the country-specific constructor. See the reference for the country-specific module - a country code of 'GB'
corresponds to the module called Geography::NationalGrid::GB.

        use Geography::NationalGrid;
        my $point1 = new Geography::NationalGrid( 'GB',
                GridReference => 'TQ 289816',
        );
        print "Latitude is " . $point1->latitude . " degrees north\n";

# DESCRIPTION

You ask for an object for the correct country, described using the ISO 2-letter country code. You will need to
supply information to the constructor. You may then call methods on that object to do whatever operations you need.
Conceptually each object represents a point on the ground, although you some grid systems may take that point to 
be a corner of a defined area. E.g. a 6-figure OS National Grid reference **may** be thought of as the point at the south-west
of a 100m by 100m square.

# METHODS

See the documentation for the country-specific module. This modules provides these generic methods which may or may not be used
by the country-specific objects:

- latitude() / longitude()

    Returns the appropriate value in floating point degrees

- easting() / northing()

    Returns the appropriate value in metres, truncated to integer metres

- data(PARAMETER)

    Access the Userdata hash in the object, and retrieve whatever is keyed against PARAMETER. Typical use might be to store
    some long information about the point, such as the site name.

- deg2string(DEGREES)

    Returns a string of the form '52d 28m 34s' when given a number of degrees. You can also call this as a class method.

- deg2rad(DEGREES)

    The input number of degrees may be in one of 3 formats: a floating point number, such as 52.34543; a reference to an array of
    3 values representing degrees, minutes and seconds, such as \[52, 28, 34\]; a string of the form '52d 28m 34s'. Returns
    the number of radians as a floating point number.  You can also call this as a class method.

- rad2deg(RADIANS)

    Converts a floating point number of radians into a flaoting point number of degrees.  You can also call this as a class method.

# OTHER COUNTRIES

The core distribution includes the GB and IE modules, allowing you to work with the National Grids of Britain and Ireland.
Adding support for another country would require the module for that country to be installed - the naming convention is
'Geography::NationalGrid::' followed by the ISO 2-letter country code, in capitals.

If you would like to provide support for another country please see the DEVELOPERS section below.

# ACCURACY

The routines used in this code may not give you completely accurate results for various mathematical and theoretical reasons.
In tests the results appeared to be correct, but it may be that under certain conditions the output
could be highly inaccurate. It is likely that output accuracy decreases further from the datum, and behaviour is probably divergent
outside the intended area of the grid, but in any case accuracy is not guaranteed.

This module has been coded in good faith but it may still get things wrong.
Hence, it is recommended that this module is used for preliminary calculations only, and that it is NOT used under any
circumstance where its lack of accuracy could cause any harm, loss or other problems of any kind. Beware!

# DEVELOPERS

This module was originally written for the OS National Grid of Great Britain, but built in a way to
allow other countries to be easily plugged in. This module is the base class; it contains a lot of the functions
that you'll need - most notably the transformations between transverse Mercator projections and Latitude/Longitude positions.
Modules can use this class and override methods as needed.

If you do write a module then why not keep the basic object interface similar to the 'GB' and 'IE' modules - for example,
why not simply inherit the latitude() accessor method from here. There will probably be country-specific methods that you
wish to add aswell, and features of the GB module may not apply to your grid.

This module contains some object methods which you can inherit, and these are data(PARAMETER), northing(), easting(),
latitude() and longitude(), and the \_mercator2latlong() and \_latlong2mercator() internal methods. All of these assume that your object
has certain pieces of data in certain places. See the METHODS section above.

In short, to write a module for a new country you simply need to write the new() routine to create a fully populated object. You
may need to write a gridReference() accessor routine, and probably need to write the routines to convert raw eastings & northings
to/from a grid reference. You'll also need the parameters of the ellipsoid used and the projection parameters. Most national grids are
transverse Mercator projections, which means you can inherit the internal conversion
routines from this class and you'll have an easy job. Otherwise
you may need to implement your own conversion.

# AUTHOR

P Kent

# CONTRIBUTORS

Matthew Somerville <matthew@mysociety.org>

# AUTHOR AND COPYRIGHT

Copyright (c) 2002 P Kent. All rights reserved.
This program is free software; you can redistribute it and/or modify it under the same terms as Perl itself.

Equations for transforming latitude and longitude to, and from, rectangular grid coordinates
appear on an Ordnance Survey webpage, although they are
standard coordinate conversion equations - thanks to the OS for clarifying.
