# asteroid_impact
Asteroid Impact is an open-source video game stimulus for conducting experimental research on human subjects.

Questions? Contact Richard Huskey (huskey.29@osu.edu)

<h1>License</h1>

Asteroid Impact was developed in the Media Neuroscience Lab (http://www.medianeuroscience.org/) Rene Weber, PI and the Cognitive Communication Science Lab (http://cogcommscience.com/) Richard Huskey, PI.

Asteroid Impact is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.

You should have received a copy of the license along with this work. If not, see http://creativecommons.org/licenses/by-sa/4.0/.

Key Contributors include:

Richard Huskey, Nick Winters, Justin Keene, Jacob Fisher, Britney Craighead, and Rene Weber

<h1>How to cite</h1>

If you use Asteroid Impact in your research, please cite:

Huskey, R., Craighead, B., Miller, M. B., Weber, R. (under review). <em>Intrinsic Reward Motivates Shifts BetweenCognitive Control and Default Mode Networks During Task Performance.</em>

We will update the citation once the manuscript is in press.

<h1>Introduction</h1>

<em>Asteroid Impact</em> is a point-and-click style video game where subjects use a cursor to collect crystal-shaped targets that are displayed at different locations while avoiding asteroids that bounce around the screen. Game difficulty is manipulated by altering the number of targets a subject needs to collect, the number of objects to be avoided, and the rate at which these objects move. The stimulus provides tremendous experimental control in that all random aspects of the game are removed; any differences in game experience are the result of player intervention. To help resolve this potential confound, the stimulus provides a high resolution content analysis of <em>all</em> events in the game (e.g., when a crystal is collected, x/y position of the player's cursor, time when player dies) with a 16ms temporal resolution. This content analysis is exported to a .csv file that allows for subsequent computation using a wide variety of analysis packages.

In terms of experimental manipulation, researchers can specify different levels of difficulty <em>a priori</em> as well as a time duration for how long a given level will last. <em>Asteroid Impact</em> also features an adaptive mode where the game automatically increases or decreases in difficulty depending on player performance. Each of these game states can be separated by a black screen or set of instructions. These different game states can be scripted together in any sequence the researcher may choose, thereby allowing the development of sophisticated experimental paradigms. The game's artwork can easily be manipulated and the game scales to fit in both full-screen and windowed modes across a variety of screen resolutions. The game is also designed to interface with TTL triggers, a feature which allows for synchronizing game states with psychophysiological and neurophysiological measurement equipment.

<em>Asteroid Impact</em> is written in Python and carries a fully open source license (CC BY-SA 4.0). This means that the game can be modified to suit the  needs of any given research lab. Moreover, the game is platform agnostic and can be used on Windows, OS X, or Linux (see known bugs below). The game requires minimal computational power and can run on low-cost computer systems. Taken together, these features provide the research community with a highly flexible tool that overcomes the issues discussed earlier. Moreover, the game conforms to the latest trends in open-science by providing a free tool that allows for replication.

<h1>Getting Started</h1>

Download:
<ul>
<li>src</li>
<li>doc/html</li>
</ul>

The html documentation files specify necessary software dependencies and how to get <em>Asteroid Impact</em> running on your system.

The src directory contains the game files.

For Mac users. In order to get Asteroid Impact to work on OS X Sierra (10.12), updated versions of PyGame and PySerial must be installed. PyGame cannot be installed through pip on Mac, so you can follow [these instructions](https://jamesfriend.com.au/installing-pygame-python-3-mac-os-yosemite). Alternately, you can use an integrated development environment (IDE) such as [PyCharm](https://www.jetbrains.com/pycharm/). 

<h1>New Features</h1>

<h2>Questionnaire Block Development</h2>

A new survey question step will display configurable text (a question) and a configurable list of answers. The player selects an answer by clicking on it with their mouse. There is a provision so that double-clicking doesn't select answers for two consecutive survey questions. The answers are recorded to a new output file.

<h2>Reaction Time Element</h2>

This is a new gameplay element where the player is expected to press a button when they see the icon and/or hear the tone as quickly as possible, and their reaction time is be recorded. From the player's point of view these icons/sounds will happen at random times. From the operator's point of view, all players see the same sequence of delays between reaction time tests. The operator can configure the starting time of each reaction test prompt, and more than one will likely appear during a single session. Each subject will see the same delays happen from the start of a step to when the reaction time prompts appear. The reaction test may prompt with one or both of: (a) graphic appearing on screen, (b) sound playing. There are 3 different reaction time prompt graphics and tones, although operators can add more. Each reaction time prompt configured in the step JSON will have a list of times at which it appears. The reaction time results will be logged in the same per-frame log file, but also optionally another log file that has just the reaction-time results.

<h2>Parallel Port Programming</h2>

On a configurable list of game events, such as the start of the gameplay step, or difficulty increasing in the adaptive gameplay mode, output a pulse of 50-100ms over the parallel port. This will be captured by another computer that is recording other subject physiological information to capture the game state transitions with the other subject information. The JSON configuration has new settings to specify where to find the parallel port, and which numbers to send to the parallel port for which desired game events. The PC running Asteroid Impact is running Windows 7 or Windows 10 and this feature does NOT work with Mac or Linux.

<h2>Extend Parallel Port to Serial Output</h2>

The above features are extended to a serial port. This adds new configuration options to configure the serial port connection, and instead of pulsing all parallel port pins at once then resetting, outputs a configurable sequence of bytes when each of the configured events occur. For serial, this won't send a second "zero" message after a delay the way it is required to trigger a pulse over parallel.

<h2>Parallel Port Trigger</h2>

This allows the operator to specify a parallel port connection and values to look for each frame to advance to the next step like how a byte over serial or a keypress can be configured now. The serial port will be checked each frame, and if the incoming value seen matches the configured value, and didn't on the previous frame, the counter will be increased. This means that incoming pulses need to be at least 2-frames wide, preferably 50milliseconds or so.

<h1>! Known Issues</h1>

As with all experimental software, please use <em>Asteroid Impact</em> at your own risk, and after sufficient testing on your own hardware. The software does not come with any warranty. 

<em>Asteroid Impact</em> Has undergone extensive testing in Windows7, Windows10, and OS X 10.10.5 environments. Known issues are listed below:
<ul>
<li>Trigger Latency: TTL triggers connected with a USB or serial port experience a small latency depending on hardware. See the "Trigger Latency" section in the HTLM documentation for more details on how to test for this on your hardware.</li>
<li>Event Timer: When using a Mac OS, the "total_millis" column in the .csv output does not tick correctly (not ticking every 16ms). This is due to a known limitation in PyGame 1.9.1 and we currently do not see a workaround for this issue. To our knowledge, the log records all game events, it is just that clock is not recording the timings correctly. For Mac users, this makes time-locked content analysis unreliable.

On Windows systems, depending on hardware and the way your .json script is configured, you may experience issues where the timing does not tick for the standard 16ms. This usually happens on a game state change (e.g., from instructions to gameplay), and only happens once. All subsequent timing ticks are 16 ms. As far as we can tell, the overall clock is still correct, so the timing files should still be accurate. However, for users who truly require 16ms precision, we recommend additional validation.</li>
<li>Parallel Port: The parallel port feature only works with Windows operating systems and was never developed to work with Mac or Linux.</li>
<li>Reaction Time: The visual and auditory reaction time prompts experience a small latency and this is hardware dependent. See the "Game Timing" section in the HTML documentation for more details on this latency and how to test this on your own hardware. Note, this is true of all reaction time software, and Asteroid Impact's latencies seem similar to other major experimental packages (see e.g., http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0067769). Still, it is recommended that users do their own testing on their own systems to validate the software.</li>
</ul>
