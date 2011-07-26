# Jsonify

[Jsonify](https://github.com/bsiggelkow/jsonify) is to JSON as [Builder](https://github.com/jimweirich/builder) is to XML.

## Goal

Jsonify provides a ___builder___ style engine for creating correct JSON representations of Ruby objects.

Jsonify hooks into Rails ActionView to allow you to create JSON view templates in much the same way that you can use Builder for XML templates.

## Motivation

JSON and XML are without a doubt the most common representations used by RESTful applications. Jsonify was built around the notion that these representations belong in the ___view___ layer of the application.
For XML representations, Rails makes this easy through its support of Builder templates, but, when it comes to JSON, there is no clear approach.

___more coming soon___

## Installation

gem install jsonify

## Usage

### Standalone
    # Create some objects that represent a person and associated hyperlinks
    @person = Struct.new(:first_name,:last_name).new('George','Burdell')
    @links = [
      ['self',   'http://example.com/people/123'],
      ['school', 'http://gatech.edu'],
    ]

    # Build this information as JSON
    require 'jsonify'
    json = Jsonify::Builder.new(:pretty => true)

    json.result do
      json.alumnus do
        json.fname @person.first_name
        json.lname @person.last_name
      end
      json.links(@links) do |link|
        {:rel => link.first, :href => link.last}
      end
    end

    # Evaluate the result to a string
    json.compile!

Results in ...

    {
      "result": {
        "alumnus": {
          "fname": "George",
          "lname": "Burdell"
        },
        "links": [
          {
            "rel": "self",
            "href": "http://example.com/people/123"
          },
          {
            "rel": "school",
            "href": "http://gatech.edu"
          }
        ]
      }
    }

### View Templates

Jsonify includes Rails 3 template handler. Rails will handle any template with a ___.jsonify___ extension with Jsonify.
The Jsonify template handler exposes the `Jsonify::Builder` instance to your template with the `json` variable as in the following example:

    json.hello do
      json.world "Jsonify is Working!"
    end

## Documentation

[Jsonify Yard Docs](http://rubydoc.info/github/bsiggelkow/jsonify/master/frames)

## Build Status

[Compliments of Travis](http://travis-ci.org/bsiggelkow/jsonify)

## TODOs
1. Consider simplified means of creating arrays (e.g. json.links(@links) {|link| ...})
1. Benchmark performance

## Roadmap

1. Split Rails template handling into separate gem
1. Add support for Sinatra and Padrino (maybe separate gems)

## License

This project is released under the MIT license.

## Authors

* [Bill Siggelkow](https://github.com/bsiggelkow)
