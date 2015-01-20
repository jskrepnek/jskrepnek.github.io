---
layout: post
title:  "AngularJS text carousel directive"
date:   2013-12-18 7:11
categories: angularjs directives
---
A primitive, non-interactive text carousel directive for AngularJS [gist](https://gist.github.com/jskrepnek/8024016).  It requires jQuery for the fading animations, but you could use pure css 3 transitions to remove that dependency.</p>

A nice extension would involve seeding the values with an array of strings on the scope.  The interface could be made to support either use case seamlessly.

See it in [action](http://embed.plnkr.co/8ReWhq/preview) at Plunker.

Here's the directive:

{% highlight js %}
app.directive('textCarousel', function ($timeout) {

    return {
        restrict: 'A',
        link: function (scope, elem, attrs) {

            var timeoutId,
                index = 0,
                values;

            values = attrs.values.split(',');

            function goToNextValue() {

                index += 1;

                if (index >= values.length) {
                    index = 0;
                }
            };

            function setCarouselText() {
                elem.text(values[index]);
            }

            function updateCarousel() {
                setCarouselText();
                goToNextValue();
                scheduleNext();
            };

            function scheduleNext() {
                timeoutId = $timeout(function () {
                    elem.fadeOut(200, function () {
                        $(this).text(values[index]).fadeIn(20);
                        updateCarousel();
                    });
                }, 2000);
            };

            updateCarousel();

            elem.on('$destroy', function () {
                $timeout.cancel(timeoutId);
            });
        }
    };

});
{% endhighlight %}

Use it in your markup like this:

{% highlight html %}
<div>
  I really like to <span text-carousel values="run, walk, bike, jog, eat, sleep, and drink"></span>.
</div>
{% endhighlight %}


