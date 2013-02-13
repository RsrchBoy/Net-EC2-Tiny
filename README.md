# NAME

Net::EC2::Tiny - Basic EC2 client

# VERSION

version 0.01

# SYNOPSIS

    use 5.014;
    use Net::EC2::Tiny;
    use Data::Dumper;

    my $ec2 = Net::EC2::Tiny->new(
          AWSAccessKey => $ENV{AWS_ACCESS_KEY},
          AWSSecretKey => $ENV{AWS_SECRET_KEY}',
          region       => $ENV{AWS_REGION},
          debug        => 1,
    );

    # We are essentially encoding 'raw' EC2 API calls with a v2 
    # signature and turning XML responses into Perl data structures

    my $xml = $ec2->send(
          Action       => 'DescribeRegions',
          RegionName.1 => 'us-east-1',
    );

    say Dumper $xml;

# OVERVIEW

This module is intended to be a quick-n-dirty glue layer between a script and some 
[Amazon EC2 API calls](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/OperationList-query.html).
Normally I'd use something like bash and curl for this, but Amazon's API signature 
requirements demand a little bit more than bash and curl doesn't support Amazon's 
signature schema.

All errors are fatal and reported via croak.

If you want to do "serious" development with the EC2 API, you probably ought to
use something like [Net::Amazon::EC2](http://search.cpan.org/perldoc?Net::Amazon::EC2) or [VM::EC2](http://search.cpan.org/perldoc?VM::EC2).

# ATTRIBUTES

## AWSAccessKey

This is the Amazon API access code. __Required__ at object construction time.

## AWSSecretKey

This is the Amazon API secret key. __Required__ at object construction time.

## debug

Set this to a true value if you want debugging information. Defaults to false.

## version

This is the AWS EC2 API version. Defaults to 2012-07-20.

## region

This is the EC2 region to which to make calls. Defaults to 'us-east-1'

## base\_url

This is the base URL used by the module to send encoded requests to. Defaults to
`https://ec2.us-east-1.amazonaws.com` This attribute uses the region attribute 
automatically.

## ua

The user agent object which sends requests. Defaults to [HTTP::Tiny](http://search.cpan.org/perldoc?HTTP::Tiny)

# METHODS

## send

This method expects key/value pair list with a required 'Action' key set to 
a valid EC2 API call.  Subsequent parameters in the list must be appropriate
parameters for the specified Action.

The request will be signed with a valid v2 signature and submitted to AWS. The
XML response will be turned into a Perl data structure using [XML::Simple](http://search.cpan.org/perldoc?XML::Simple) 
and returned.

# SUPPORT

Please report any bugs or feature requests to "bug-net-ec2-tiny at
rt.cpan.org", or through the web interface at
[http://rt.cpan.org/NoAuth/ReportBug.html?Queue=Net-EC2-Tiny](http://rt.cpan.org/NoAuth/ReportBug.html?Queue=Net-EC2-Tiny).  I will
be notified, and then you'll automatically be notified of progress on
your bug as I make changes.

Or, if you wish, you may report bugs/features on Github's Issue Tracker.
[https://github.com/mrallen1/Net-EC2-Tiny/issues](https://github.com/mrallen1/Net-EC2-Tiny/issues)

# SEE ALSO

This library is pretty useless without the EC2 API docs.

- [EC2 API docs](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/OperationList-query.html)

# AUTHOR

Mark Allen <mrallen1@yahoo.com>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2013 by Mark Allen.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
