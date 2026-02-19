## Key Takeaways
%%Bullet points that quickly summarize what was accomplished%%
- Added multicolor LEDs to our existing DHT11 LCD circuit.
- Adjusting the code to show data on the LEDs was challenging to tune but not to implement (thanks to the "AI").
- Adding in test functions into code can help when making adjustments to other functions within the code.

***
#### In-Depth
%%A more informative summary of what was accomplished%%
This lab was adding on more to our DHT11 LCD Display circuit by including two WS2812B LEDs to show different colors based on the temperature/humidity. The challenging part of this lab was the coding side since I wanted to have a color gradient based on the data. This wasn't too challenging since I had the LLM create the code based off of my suggestions. I also had to do some tuning to make sure that the brightness/gradient was the way I wanted it to work. I also added in a "test" function into the code that would run both LEDs through their gradient range every 8 seconds (every 4 times that the data was updated). This helped to adjust the colors I wanted in the gradient since it was difficult to fluctuate the temperature of the sensor. 

***
## Setup
%%What circuits/logic/programming was built for the project? How was the hardware set up?%%

#### Schematics

![[DHT11 LED Schematic - 2305.png]]%%**Add kicad schematic here**%%

#### Circuit Build

%%**Phone pictures**%%

***
## Measurements/Results
%%Results/images from the end result of the testing process. Could also be whether or not the problem was solved.%%

%%**Function photos and video**%%


***
## Notes
%%Commentary/'the story' of how the lab went (what issues there were, how did you do your testing, what did you learn.)%%
##### Problems
Initially the issue I was running into was the GDB server not wanting to connect to the pico. I never really figured out what was happening but the solution was to remove the pico from the prototyping board and start the debug with it removed. For some reason once it had successfully connected I was able to reinstall the pico on the proto board and reconnect to it. It would only stop connecting when VS Code was restarted.

The other issue I ran into was the DHT11 module not communicating with the pico. I never really figured out why but eventually it started working again after I had spent some time making sure the LCD was working correctly (using some test code). My guess is that since the modules we are using are very cheap they would have some flakiness due to the cost point they were designed to hit. 

##### Code
This is some of the code that the "AI" created when I asked for color gradients. The adjustments that I made were primarily in the equations used to calculate the color values. I needed to adjust the values because some of the colors were rather overpowering and made it harder to see changes in the gradient.

```
// Convert humidity (0-100%) to color gradient: red (0%) to green (100%)

uint32_t humidity_to_color(float humidity) {

    // Linear interpolation from red to green (20-50% range)

    uint8_t green = ((255 * 0.5) * (1.0 - (humidity - 20) / 30.0));//(uint8_t)

    uint8_t red = ((255 * 0.2) * (((humidity - 20) / 30.0)));

    uint8_t blue = 0;//(uint8_t)

    return urgb_u32(red, green, blue);

}
```

##### Testing Process
Testing was pretty straightforward but it was a little time consuming since I was making adjustments to the LED color equations to get the brightness I wanted. My general process was to watch the humidity LED change after placing my finger over the DHT11 sensor and comparing that to the percentage shown on the LCD. Eventually I decided to have the "AI" add in some code that would show the gradient every 8 seconds just so that it would be easier for me to tune the colors.  It also had the added benefit of showing that the LEDs were working.

```
        // Every 4 updates, run 2-second animation

        update_count++;

        if (update_count >= 4) {

            animate_gradients(pio, sm, 1500);

            update_count = 0;
```

This was contained in a larger loop that would update the LCD and the data recorded from the DHT11 every 2 seconds.

%% Example headings


##### Solutions
##### Results
##### Code
##### Changes
%%

***

%%Commented section for unformatted notes



%%