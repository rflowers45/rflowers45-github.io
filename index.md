# Robert Flowers Computer Science Portfolio
Hi, I'm an aspiring software engineer and computer science student at Weber State.

I'm a team guy; I like solving problems with my team and leading them to the finish line!

Below are some projects I've built.

## Projects
### Stock Investing
_____________________________

![Stock Investing Application](/docs/assets/Investing.png)

An investment simulation where the user is given 7 random days and $10,000 in cash in an attempt to make a profit.

***Technology/Langauges Used***

.NET 6 framework, Razer Pages format, Javascript, JQuery, and C#.

***Challenges***
- Displaying a chart
- Linking 3 interactive UI elements together
- Asynchronous API Calls

***Solutions***

- Displaying a chart proved to be tricky but doable. I was able to use a JQuery libray to create a pie-like chart out of a radial slider. Below is the code I used to implement this:
```
<script src="https://cdn.jsdelivr.net/npm/round-slider@1.6.1/dist/roundslider.min.js"></script>
<script src="//code.jquery.com/ui/1.11.4/jquery-ui.js"></script>
//<!--CHART-->
        var price = 100;
        $("#doughnut").roundSlider({
            tooltipFormat: "DoughnutVal2",
            sliderType: "min-range",
            handleSize: "+1",
            handleSize: "24,12",
            handleShape: "square",
            radius: 100,
            width: 15,
            max: price,
            value: 1,
            step: 1
        });
```
- Linking 3 interactive UI elements, including the chart, proved to be even more difficult. With some time and thought, I was able to link a text-field, slider, and chart together to provide several options for the user to select the amount of shares they would like to purchase. The code below illustrates this:
```
//<!--SLIDER LINKING-->
        $(function() {

            $("#slider").slider({
                min: 1, max: 99, step: 1, value: 0,
                slide: function(event, ui) {
                    var price = (ui.value * 100);
                    $("#shareCount").val(ui.value); //Linking text field with slider
                    $("#doughnut").roundSlider("setValue", ui.value); //Linking chart with slider
                }
            });
            var initialValue = $("#slider").slider("option", "value");
            $("#shareCount").val(initialValue);
            //Linking slider & chart with text field
            $("#shareCount").change(function() {

                var oldVal = $("#slider").slider("option", "value");
                var newVal = $(this).val();
                var price = (newVal * 100); //for testing
                if (isNaN(newVal) || newVal < 1 || newVal > 100) {
                    $("#shareCount").val(oldVal);
                } else {
                    $("#slider").slider("option", "value", newVal); 
                    $("#doughnut").roundSlider("setValue", newVal); //Linking chart with slider
                }
            });
            $("#doughnut").change(function(e) {
                var shares = (e.value / 100) //This is for converting the price into a share.
                $("#slider").slider("option", "value", e.value);
                $("#shareCount").val(e.value); //Linking chart with slider

            });
        });
```
### Boggle
_____________________________

### Banking
_____________________________

### Hangman
_____________________________
