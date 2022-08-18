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
- On the backend, I was able to pull stock history data from an API and display it to the user via AJAX. Below is one of the functions I wrote to retrieve the initial stock data:
```

        //Retrieves stock data in a clump and returns the data
        public IActionResult OnPostGetStocks(string value)
        {
            try
            {
                //*********API CALL*************
                var symbol = value; //Setting the ticker symbol to what the user has entered
                var apiKey = "##########"; // API Key has been redacted
                var dailyPrices = $"https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol={symbol}&outputsize=full&apikey={apiKey}&datatype=csv"
                    .GetStringFromUrl().FromCsv<List<StockData>>();
                //*********API CALL*************

                //This fills the object with values
                dailyPrices.PrintDump();

                int lastIndex = dailyPrices.Count() - 1; //This is used to get the total amount of indices in the list. For random generation
                var testDate = GetRandomDate(lastIndex); //Getting a random indices for closing stock price
                string dateString = dailyPrices[testDate].Timestamp.ToString();
                
                //This grabs the objects day
                var dayPrice = dailyPrices[testDate].Close;//This gets the price
                price = dayPrice; //Setting the global price to = the day price
                string displayResults = ("Getting data for " + value + "<br> Date: " + dateString + "<br> Closing price: $" + dayPrice);
                //SESSION VARIABLES
                HttpContext.Session.SetString("balance", "10000");
                HttpContext.Session.SetString("price", price.ToString());
                HttpContext.Session.SetInt32("shares", 0);
                HttpContext.Session.SetInt32("currentDay", testDate);
                HttpContext.Session.SetInt32("dayCounter", 1);
                HttpContext.Session.SetString("ticker", value);

                return new JsonResult(displayResults);

            }
            catch (Exception)
            {
                return new JsonResult("Ticker symbol not found or random date is not a trading day. Try again.");

            }

        }
```
I felt very confident with this project and spent a large amount of time dedicated to it. I'm glad I had the opportunity to build this application!

### Boggle
_____________________________

![A photo of a boggle game interface](/docs/assets/Boggle.png)

The goal of this project was to create a boggle (or boggle-like) game where players select letters from a grid to form a word. The players have a limited amount of time, usually 60-90 seconds, to form as many words as they can. Words are scored based on length. The game requires at least 2 people to participate and results are dispayed at the end of the game.

***Technology/Langauges Used***
.NET 6 framework, MVC format, SignalR, Javascript, JQuery, and C#.

***Challenges***
- Multiple Players
- SignalR Usage
- Asynchronous Timer

***Solutions***
- The feature of having multiple players proved to be the most challenging aspect of this project. With enough research, I was able to successfully incorporate multiple players into the game. Below is some code providing an idea of how this was accomplished:
```
        //Keeping track of players
        public static List<string> playerID = new List<string>();
        public static int playerCount = 0;
        public static List<string> player_one_words = new List<string>();
        public static List<string> player_two_words = new List<string>();
        public static int player_one_score = 0;
        public static int player_two_score = 0;

        //Checking for player count
        public override async Task OnConnectedAsync()
        {
            await Clients.All.SendAsync("HideBoggle");
            playerID.Add(Context.ConnectionId);
            playerCount++;
            await base.OnConnectedAsync();
            if (playerCount < 2)
            {
                await Clients.All.SendAsync("DisableButtons");
            }
            else if (playerCount == 2)
            {
                await Clients.All.SendAsync("EnableButtons");
            }            
        }
```


### Banking
_____________________________

***Technology/Langauges Used***
.NET 6 framework, MVC format, Entity Framework, Javascript, JQuery, and C#.

***Challenges***
- 
- 
- 

***Solutions***
### Hangman
_____________________________
