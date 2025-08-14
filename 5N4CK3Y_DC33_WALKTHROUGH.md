# The AND!XOR 5n4ck3y DEF CON 33 Walkthrough – Badge Enabled Non Directive Enigma Routine (BENDER)

Welcome to our 10th year of building indie electronic badges for hacker summer camp shenanigans. This year’s badge features a Pepper’s Ghost hologram, custom acrylic, and mirrors. Unlike traditional CTFs, our contest starts with a mystery: contestants receive a `.z5` file, which is a Z Machine Interactive Fiction game. Once loaded in a Z Machine emulator (like dfrotz), players enter a text-based world inspired by Zork, with 84 unique areas, dozens of NPCs, and artifacts. We call this format Badge Enabled Non Directive Enigma Routine (BENDER) - a text adventure CTF that blends digital puzzles with real-world hardware hacking.

The goal: OSINT your way through BENDER’s digital world, discover the challenges, solve them, and interact with 5n4ck3y-the IoT Hacked Vending Machine. Many challenges require direct interaction with 5n4ck3y and its peripherals, while others are offline to avoid excessive lines. Solve enough challenges, and 5n4ck3y vends a badge. You can stop there or continue for more points.

Challenges are split into two phases (Totalling 36 challenges)
- **Phase 1 (Pre-Badge):**
    - Players must verify physically at 5n4ck3y by entering a flag shown on its screen.
    - This unlocks pre-badge challenge flag entry on CTFd.
    - Complete 5 out of 17 challenges to earn a badge; a vend code is emailed to you.
    - Challenges are only found in the BENDER game; CTFd is for flag entry and scoring.
- **Phase 2 (Post-Badge):**
    - Use an AES256 decryption key (released Friday) to unlock the full game on your badge hardware.
    - Verify badge ownership by entering a code from the game into CTFd.
    - Enter the badge-displayed flag in CTFd to unlock Phase 2 challenges, providing an addition 19 challenges.

## 5n4ck3y Peripheral & Badge Attack Vectors

Many challenges involve interacting with and hacking 5n4ck3y’s peripherals, which include:
- **Hex Keypad:** Enter codes (hex, emoji, or custom icons) to trigger events.
- **NFT Printer:** Prints NFT QR codes with encoded messages.
- **HAL Eye:** Blinks Morse code via a giant red light.
- **IR Receiver:** Captures and decodes infrared signals (e.g., TV remotes, AC Controllers).
- **Audio Output:** Plays WAV/MP3 files (mic disabled for privacy).
- **Software Defined Radio:** Transmits on 915MHz ISM band (POCSAG/FLEX, FSK, SSTV, etc.).
- **LoRA Radio:** Meshtastic BBS interface.
- **RGB LEDs & Display:** Visual feedback and messaging.
- **Core Processing Unit:** Runs BBS, controls peripherals, and the 5n4ck3y LLM.

Many challenges involve interacting with and hacking the badge hardware:
- **RP2040 MCU:** Use OpenOCD to dig through the memory and bootloader for hidden flags.
- **Winbond Flash:** Use a chip clip and hardware tools to dump badge firmware and search for embedded flag files.
- **Hidden UART:** Connect to hidden UART pins and use a serial terminal to uncover secret flags.
- **SPI Bus Sniffing:** Monitor SPI communications between the MCU and LED peripherals to extract hidden data and flags.
- **Trace Cutting:** Desolder battery holders and cut specific PCB traces to trigger new badge behaviors and reveal flags.
- **Trace Soldering:** Desolder battery holders and bridge broken pads or traces under battery holders to enable hidden badge features and unlock flags.
- **Hall Effect Sensor:** Place a magnet near the badge’s Matt Damon face to activate a sensor and display a flag.
- **Thermistor:** Heat or chill the badge’s temperature sensor to change the badge state.
- **Silkscreened Hidden Messages:** Disassemble the badge and inspect under the screen ribbon for secret messages.
- **Ground Plane Hidden Messages:** Closely examine the badge PCB’s ground plane for etched or silkscreened clues leading to flags.

The contest blends digital and physical puzzles, requiring both software skills and hands-on hardware hacking. Flags are scored in CTFd, and challenge names reference their in-game locations (e.g., “Sewer,” “Riviera Conference Hall”). Play the BENDER `.z5` file in your emulator, explore, solve, and interact with 5n4ck3y to win.

## BENDER ##

BENDER is a text based CTF front which masquerades as a text based adventure game, similar to Zork, Collosal Cave, etc. The player interacts by typing commands for navigating the world map, taking items, using items, and looking at things/places. It has evolved in many ways over the years. This year we are using an Inform6 form, PunyInform, as the framework. Essentially the hacker is performing OSINT on an entire game world, to figure out what the challenges are, how to solve them, and then solving them. The game is designed to be played in a Z Machine emulator, such as dfrotz, which is available on most platforms. The point value of each challenge was determined by the 5n4ck3y LLM, if makes it more difficult to have scoring ties, in addition to if you do not like it...take it up with 5n4ck3y.

### World Design
* Generally the world is Las Vegas, from a grungy hackers point of view, with a movie-esq cyber punk exaggeration (e.g. Hackers).
* Phase 1 pre-badge will be Las Vegas in the 90s. Player is spending most of their time getting thru Vegas to the Riviera. Current map rooms contain:
    * HackRCade (Starting Area with 5n4ck3y NPC)
    * Sketchy Alleyway
    * Dead End Dumpsters
    * Vegas Sewer Tunnels Entrance
    * Sewer Maze of 11 Rooms (used to thwart AI and bots, and confuse players)
    * Vegas Sewer Tunnels Exit
    * Center Street
    * East Street
    * West Street
    * Blockbuster Video
    * Parking Lot
    * Gas Station -> becomes Smoldering Ruins of Gas Station once explosion is triggered
    * Vegas Strip North
    * Vegas Strip North Central
    * Vegas Strip South Central
    * Vegas Strip South
    * Seedy Club
    * Tatoo Parlor
    * Pawn Shop
    * Ramen Noodle House
    * Riviera Hotel Lobby
    * Riviera Casino Floor
    * Riviera Bad & Lounge
    * Riviera Hotel Conference Room
    * Riviera Hotel Elevator
    * Riviera Pool on the Roof
* Phase 2 post badge will be Las Vegas in current 2025 time. The primary goal for the player in this phase, beyond solving challenges, will be to locate 5n4ck3y, who has gone missing from the HackRCade and is rumored to be somewhere within the DEF CON 33 Contest Area at the LVCC. Current map locations include:
    * HackRcade (5n4ck3y is missing from here)
    * Sketchier Alleyway
    * Street East
    * Street Center
    * Parking Lot
    * EV Charging Station (formerly Gas Station)
    * Dead End Dumpsters Trash
    * Analog Antiques Store (formerly Blockbuster Video)
    * Street West
    * LVCC Loop South Entrance (formerly Vegas Sewer Tunnels Entrance)
    * Vegas Strip South
    * Vegas Strip South Central
    * Vape Shop (formerly Seedy Club)
    * Vegas Strip North Central
    * Vegas Strip North
    * Circus Circus Lobby 
    * Circus Circus Food Court
    * Circus Circus Fog Maze of 16 Rooms (used to thwart AI and bots, and confuse players)
    * LVCC Loop North Entrance (formerly Pawn Shop)
    * Dive Bar (formerly Tattoo Parlor)
    * Ramen Noodle House
    * Vegas Sphere
    * High Roller Ferris Wheel
    * Top of the High Roller (where you are stuck when it breaks down)
    * LVCC West Entrance
    * DC33 Merch Line Entrance
    * LVCC Food Court
    * LVCC Main Hall
    * LVCC South Entrance
    * DC33 Contest Area (Player needs to find 5n4ck3y here)
    * DC33 Talk Tracks
    * DC33 Merch Line  LineCon (a series of 20 one-way rooms)
    * The End of the Line (merch booth)

### Challenges
Put your challenge title here, walkthrough for solving, location in BENDER, and flag. This section will be used post-con when sharing the answer. Right now just dumping ideas for challenges.

* Phase 1 - 1 LoRA Meshtastic BBS - Hak Tak Toe (Wireless & AppSec)
    * **Overall:** Player interacts with a LoRA Meshtastic BBS via 5n4ck3y, plays a game of "Hak Tak Toe," and exploits it with a buffer overflow to get a flag.
    * **Phase 1: Uncovering the Path (In-Game BENDER - Puzzle, Observation)**
        *   **Location:** "Radio" object (in "Riviera Pool On The Roof"). "UV Flashlight" (in "Ramen Noodle House").
        *   **Description:**
            1.  Player finds a "Radio" object at the "Riviera Pool On The Roof". Its description mentions "Laura" and a nearly invisible message from lemons on the bar.
            2.  Player finds and takes the "UV Flashlight" from the "Ramen Noodle House".
            3.  Player uses the "UV Flashlight" on the "Radio" object.
        *   **Key Objects Required:** "UV Flashlight".
        *   **Output/Hint:** Revealed message on the Radio: "For a mediumfast time message 5n4ck3y to mesh up and ask... ? ...NDQzNU1BVFREQU1PTg== (4435MATTDAMON)...Whatever that means. " This has 5n4ck3y print an NFT QR code which can be loaded by Meshtastic to configure the radio for our channel.
    * **Phase 2: Meshing with 5n4ck3y (Wireless - LoRA Meshtastic, AppSec)**
        *   **Action:** Player uses a LoRA Meshtastic device to interact with 5n4ck3y.
        *   **Description:**
            1.  Player connects to the 915 MHz LoRA Meshtastic network.
            2.  Following the hint from the Radio, player sends a Direct Message (DM) containing only "?" to 5n4ck3y's Meshtastic node.
            3.  5n4ck3y responds with an interactive menu on the player's Meshtastic device, offering options such as "Mail" and "Games".
                *   Exploring "Mail" reveals an "Administrator Security Bulletin". Persistently attempting to read this bulletin progressively displays parts of a CVE description. This CVE details a critical vulnerability in the "Hak Tak Toe" game related to handling extremely large numerical inputs, strongly hinting that a maximum unsigned integer (e.g., 4,294,967,295) could trigger an overflow.
            4.  Player navigates the menu to "Games" and selects "Hak Tak Toe" to initiate the game.
            5.  Player needs to input a move. To exploit the game (as hinted by "great numbers have limits" from the radio and the CVE details from the mail bulletin), player sends the maximum unsigned integer value (`4294967295`) as their move.
            6.  This input causes a buffer overflow in the game logic on 5n4ck3y's side. Buffer underflow works as well, since it is unsigned and essentially the same thing.
        *   **Key Objects Required:** IRL LoRA Meshtastic capable device.
        *   **Output/Hint:** The flag is displayed/sent back via Meshtastic after the successful exploit.
        *   **5N4CK3Y INTERACTION:** 5n4ck3y is the LoRA Meshtastic node, hosts the interactive menu, mail system, the Hak Tak Toe game, and is the target of the buffer overflow.
    * **Flag:** `flag{m35h7ash@kt@kt03}`
    * **CTF Points (Hard):** 88
    * **CTFd_Challenge_Name:** `H4k_T4k_T03`

* Phase 1 - 2 LoRA Meshtastic BBS - Hot Cold Location Foxhunt (Wireless, OSINT, & GPS Spoofing)
    * **Overall:** Player engages in a GPS-based scavenger hunt via 5n4ck3y's LoRA Meshtastic BBS, requiring OSINT to identify a target location from a photo and then "reach" it (potentially via GPS spoofing) within a time limit.
    * **Phase 1: The Starting Line (In-Game BENDER - Puzzle, Observation)**
        *   **Location:** "Radio" object (or another designated object in "Riviera Pool On The Roof"), "UV Flashlight" (in "Ramen Noodle House"), "Keyboard" (in "Rio Conference Room").
        *   **Description:**
            1.  Player finds an object in the "Riviera Pool On The Roof" that has a message written in lemon juice (e.g., the "Radio" or another item as per original notes: "Add some other object to the pool area that must be SHINED on by the UV Light").
            2.  Player finds and takes the "UV Flashlight" from the "Ramen Noodle House".
            3.  Player uses the "UV Flashlight" on the designated object at the pool to reveal a hint about the "Hawt & Chill" game and the need for a password.
            4.  Player finds a "Keyboard" in the "Rio Conference Room". Examining it (or looking under it) reveals the password "pl3as3" to access a local BBS game.
        *   **Key Objects Required:** "UV Flashlight".
        *   **Output/Hint:** Password: "pl3as3". A message instructing the player to connect to the Meshtastic BBS and use the password to start the "Hawt & Chill" game.
    * **Phase 2: The Chase (Wireless - LoRA Meshtastic, OSINT, GPS Spoofing)**
        *   **Action:** Player uses a LoRA Meshtastic device to interact with 5n4ck3y's BBS.
        *   **Description:**
            1.  Player connects to the 915 MHz LoRA Meshtastic network.
            2.  Player interacts with 5n4ck3y's BBS and provides the password "pl3as3" to start the "Hawt & Chill" GPS scavenger hunt.
            3.  The BBS provides the player with a 5n4ck3y code: `915906`.
            4.  Player inputs this code into 5n4ck3y (the physical vending machine), which then "prints" an NFT (e.g., a slip of paper with a QR code of an URL). This NFT links to a photo on Dropbox depicting "Zapp & Bender." Player must use OSINT to determine the location depicted in the photo (Rio Hotel, identifiable by background details or the DC20 badges Zapp & Bender are wearing from the DEF CON documentary...or if they are cool enough recognize the Rio simply from the architecture in the background since shit hasnt changed since then). https://www.dropbox.com/scl/fi/9d69x0gq6aeaw9rq97ire/x.png?rlkey=a99sbajz7yo6g20o5lnqdjxpk&st=vwqm75f1&dl=0
            5.  The BBS instructs the player to go from 5n4ck3y's current location, start the game as a check in to verify proximity, then head to the identified photo location (Rio) within 30 minutes and "check in" via the mesh by which sends their GPS coordinates on the back end.
            6.  Player needs to transmit GPS coordinates corresponding to the target location (Rio) via their Meshtastic device. This might involve physically going there or, more likely for a CTF, GPS spoofing. This can be done the hard way by getting a few friends with radios to extend the mesh to reach the Rio and check in... or manually override the Meshtastic GPS settings via CLI or phone app (hopefully the latter but we respect people who went the mesh extension route).
            7.  The BBS provides "hawt/warm/chill" feedback based on the transmitted GPS coordinates to tell them if they are getting closer or further away from the target location. Hopefully they script this.
            8.  Upon successful "arrival" (correct GPS coordinates transmitted) within the time limit, the BBS provides the flag.
        *   **Key Objects Required:** IRL LoRA Meshtastic capable device (with GPS or not, just spoof it). Internet access for Dropbox link and OSINT. Interaction with 5n4ck3y to redeem the code.
        *   **Output/Hint:** The flag, sent via Meshtastic upon successful completion.
        *   **5N4CK3Y INTERACTION:** 5n4ck3y (as a LoRA Meshtastic node) hosts the "Hawt & Chill" game, provides the code, and validates GPS check-ins. 5n4ck3y also processes the code to dispense the NFT link.
    * **Flag:** `flag{hawtd0gch1ll1d0g}`
    * **CTF Points (Hard):** 97
    * **CTFd_Challenge_Name:** `H4w7_&_Ch1ll`

* Phase 1 - 03 POCSAG/FLEX (Wireless, RF Sniffing, POCSAG/FLEX Demodulation and Decoding, Audio Reversal)
    * **Overall:** Player finds a pager and battery in-game. The pager displays a partial message and has an FCC ID hinting at a frequency. Player sniffs RF signals at that frequency to capture the full pager message, which includes a phone number. Player calls this real-world phone number, hears a reversed voicemail, reverses the audio to get a 5n4ck3y code, and enters this code on the physical 5n4ck3y to get the flag.
    * **Phase 1: Pager Revival & The Frequency Hint (In-Game BENDER - Exploration, Item Use)**
        *   **Location:** "Motorola pager" found in "Street West". "Pager Battery" found in "SewerMazeTM11".
        *   **Description:**
            1.  Player finds a "Motorola pager" in "Street West".
            2.  Examining the pager reveals its battery is dead or missing and its screen is cracked. On the back, an FCC ID sticker is visible, and part of it clearly indicates its certified to operate in the ISM band.
            3.  Player needs to find a "Pager Battery" (Key Object) in "SewerMazeTM11".
            4.  Player uses the "Pager Battery" with the "Motorola pager" (e.g., `PUT BATTERY IN PAGER`). The pager's description then updates or an event occurs: "The pager sputters to life. The cracked screen flickers, displaying: 'Yo, hit me back, urgent! Need to tell you what's going down in the 925...'. The rest of the message, including any callback number, is unreadable due to the severe cracks."
        *   **Key Objects Required:** "Pager Battery".
        *   **Output/Hint:** A cryptic message on the pager screen. A clear frequency hint ISM band and the 921 helps hone them in on its not just anywhere in 903-928 MHz, its near 925 MHz...
    * **Phase 2: Sniffing the Airwaves & The Number (IRL RF Sniffing - POCSAG, OSINT)**
        *   **Action:** Player uses an SDR (Software Defined Radio) to monitor near 925 MHz frequency for POCSAG pager signals.
        *   **Description:**
            1.  Based on the 915.0 MHz hint from the pager, the player uses an SDR and appropriate software (e.g., `multimon-ng`) to listen for and decode POCSAG pager signals.
            2.  i.e. Open GQRX, tune your SDR to the right frequency, pipe output over UDP, and `$ nc -l -u -p 7355 | sox -t raw -esigned-integer -b 16 -r 48000 - -esigned-integer -b 16 -r 22050 -t raw - | multimon-ng -t raw -a POCSAG512 -a POCSAG1200 -a POCSAG2400 -a FLEX -f alpha -`
            3.  A repeating pager message is being broadcast (simulated or actually transmitted if 5n4ck3y can do this).
            4.  The decoded POCSAG message will contain the full text, including the phone number: POCSAG1200: Address: 9980002 Function: Alphanumeric Message: "<<<TRANSMISSION FROM: MATT.DAMON@CYBERMAIL.BBS>>> heyy. ur needed back in the loop. drop me a dime on my new Nokia @ 33 444 4 44 8 666 44 333 444 888 33 8 9 666 666 44 8 44 777 33 33 7777 444 99 33 444 4 44 8 33 444 4 44 8 33 444 4 44 8. if i'm not there, my voicemail has a special secret... and you could sing me a poem (or don't. i'm not the boss of u). we just need shit 2 listen 2 after hours...poems, songs, heavy breathing...u won't get leaderboard points for that part but it'll make us happy."
            5.  The message has phonetic T9 encoding of the number to call. Hitting three twice is E, four three times is I... EIGHT OH FIVE TWO OH THREE SIX EIGHT EIGHT EIGHT.
        *   **Key Objects Required:** SDR, software for POCSAG decoding (e.g., `multimon-ng`) or a Flipper Zero using its POCSAG demodulation & decoding capabilities.
        *   **Output/Hint:** The full pager message, including the phone number `(805) 203-6888` and the alias "Matt Damon".
        *   **5N4CK3Y INTERACTION:** 5n4ck3y is a wireless source of the POCSAG broadcast
    * **Phase 3: The Reversed Voicemail (IRL Phone Call, Audio Reversal)**
        *   **Action:** Player calls the real phone number `(805) 203-6888` obtained from the decoded POCSAG message.
        *   **Description:**
            1.  Player calls `(805) 203-6888` using a real phone.
            2.  They will hear a voicemail message that is intentionally played in reverse.
            3.  Player needs to record this reversed audio.
            4.  Player uses audio editing software (e.g., Audacity, online tools) to reverse the recorded message.
            5.  The correctly reversed message will clearly state the 5n4ck3y code: "The code for 5n4ck3y is five four eight seven six." (or similar phrasing for `54876`).
        *   **Key Objects Required:** A phone to make the call, audio recording capability, audio editing software.
        *   **Output/Hint:** The 5n4ck3y code `54876` obtained from the reversed voicemail.
        *   **5N4CK3Y INTERACTION:** None for this phase.
    * **Phase 4: The Final Vend (IRL Vending Machine Interaction)**
        *   **Action:** Player inputs the 5-digit code (`54876`) obtained from the reversed voicemail into the physical 5n4ck3y vending machine keypad.
        *   **Description:**
            1.  Upon entering the correct code `54876` on 5n4ck3y, the machine dispenses the flag (i.e. prints an NFT with the flag).
        *   **Key Objects Required:** Access to the physical 5n4ck3y vending machine.
        *   **Output/Hint:** The flag.
        *   **5N4CK3Y INTERACTION:** Player enters the code `54876` on 5n4ck3y's keypad. 5n4ck3y dispenses/reveals the flag.
    * **Flag:** `flag{R3v3r53Ph0n3H0m3}`
    * **CTF Points (Hard):** 89
    * **CTFd_Challenge_Name:** `Fl3x_Y0ur_P0574l_C0d3_Mu5cl3`

* Phase 1 - 04 Shakespere (Shakespeare Programming Language)
    * **Overall:** Player encounters an NPC, Lintile, who desires Malort. Upon receiving it, Lintile takes a swig, returns the bottle, and then recites a script written in the esoteric Shakespeare Programming Language. Player must execute this script to get one flag, then analyze and modify it to reveal a second flag.
    * **Phase 1: Discovering the Bard's Code (In-Game BENDER - NPC Interaction & Script Acquisition)**
        *   **Locations:**
            *   NPC "Lintile" in `Riviera Hotel Elevator`.
            *   "Bottle of Malort" in `SewerMazeTM04`.
        *   **Description:**
            1.  Player encounters an NPC named "Lintile" in the `Riviera Hotel Elevator`. When approached or examined, Lintile simply screams "MALORT!"
            2.  Player explores and finds a "Bottle of Malort" in `SewerMazeTM04`.
                *   *Malort Description:* "A dusty, unassuming bottle of Jeppson's Malort. The label, adorned with a vaguely menacing shield, promises a 'challenging' taste experience. Its infamous aroma-a potent blend of grapefruit rind, burnt rubber, and existential despair-wafts gently from the cap. The choice drink of hackers everywhere."
            3.  Player must take the "Bottle of Malort" and give it to Lintile (e.g., `GIVE MALORT TO LINTILE`).
            4.  Upon receiving the Malort, Lintile's demeanor changes. He snatches the bottle, takes a long, appreciative swig, shudders dramatically, and then hands the bottle back to the player. He then clears his throat and begins to passionately recite the full text of the `veiled_soliloquy` Shakespeare Programming Language script.
            5.  The player now has the script (via Lintile's recitation). The Malort bottle remains in the player's inventory. A subtle hint might be given by Lintile after the recitation, or through examining the (still mostly full?) Malort bottle, suggesting the recited text is more than just a play, perhaps mentioning "dramatic programming" or "the language of the Bard," and the need for an interpreter.
        *   **Key Objects Required:** "Bottle of Malort" (used in interaction but not consumed).
        *   **Output/Hint:** The full text of the `veiled_soliloquy` script (recited by Lintile) and clues suggesting it's an esoteric programming language.
        *   **5N4CK3Y INTERACTION:** None for this phase.
    * **Phase 2: The First Act - Standard Execution (Offline - Interpretation)**
        *   **Action:** Player uses a Shakespeare Programming Language interpreter to run the `veiled_soliloquy` script as found.
        *   **Description:**
            1.  Player identifies the script (obtained from Lintile) as Shakespeare Programming Language.
            2.  Player obtains and uses an interpreter (e.g., `shakespearelang` Python package).
            3.  Running the script `veiled_soliloquy` unmodified will output the first flag.
        *   **Key Objects Required:** The `veiled_soliloquy` script content, a Shakespeare Programming Language interpreter.
        *   **Output/Hint:** First Flag: `FLAG{2B||!2B}`.
        *   **5N4CK3Y INTERACTION:** None for this phase.
    * **Phase 3: The Director's Cut - Modified Execution (Offline - Code Analysis & Modification)**
        *   **Action:** Player analyzes the `veiled_soliloquy` script, modifies a specific conditional statement, and re-runs it with the interpreter.
        *   **Description:**
            1.  Player examines the script's logic, particularly conditional statements.
            2.  The script contains the line: `If so, Let us proceed to scene III.` (End of Act I, Scene I).
            3.  Player modifies this line to `If not, Let us proceed to scene III.` (changing "so" to "not"), as hinted by the structure of Shakespearean conditionals or by trial-and-error based on the "fork" nature of the play.
            4.  Running the modified script with the interpreter will lead to a different execution path and output the second flag.
        *   **Key Objects Required:** The `veiled_soliloquy` script content, a text editor, a Shakespeare Programming Language interpreter.
        *   **Output/Hint:** Second Flag: `FLAG{1TWasGR33K2M3}`.
        *   **5N4CK3Y INTERACTION:** None for this phase.
    * **Flags:** 
        *   `flag{2B||!2B}`
        *   **CTF Points (Medium):** 42
        *   `flag{1TWasGR33K2M3}`
        *   **CTF Points (Medium):** 58
    * **CTFd_Challenge_Name_1:** `V31l3d_S0l1l0quy`
    * **CTFd_Challenge_Name_2:** `V31l3d_S0l1l0quy_D1r3c70r5_Cu7` 

* Phase 1 - 05 5n4ck3y BENDER Hidden Room (Reversing Z Machine Code)
    * **Overall:** A flag is hidden within a normally inaccessible room in the Z-machine game file, rewarding players who reverse-engineer the game code.
    * **Phase 1: Code Diving (Reverse Engineering)**
        *   **Location:** Not a physical in-game location, but within the game's `.z5` file.
        *   **Description:**
            1.  The game contains a "purgatory" room object compiled in that is not reachable through normal gameplay.
            2.  This room's description or an object within it contains an encoded flag: `0x2b2d35333f3c3414262523763d3a7778340c29`.
            3.  Players must obtain the BENDER `.z5` game file.
            4.  Players use Z-machine disassembly tools or analysis techniques to find the hidden room and extract the encoded flag. Standard string searching might be difficult due to ZSCII encoding.
    * **Phase 2: The Hint (In-Game Interaction)**
        *   **Location:** Vegas Strip South (NPC: Cypher) and Sewer Maze TM01 (Object: Red Pill).
        *   **Description:**
            1. In **Vegas Strip South**, the player encounters an NPC named **Cypher**. Cypher is a disheveled hacker who mumbles incoherently. However, if the player listens carefully, they can make out the phrase: **"red pill."**
            2. In **Sewer Maze TM01**, the player finds a mysterious **Red Pill** object. Its description reads:  
               *"A small, translucent red pill. It seems to shimmer faintly in the dim light."*
            3. The player must take the **Red Pill** to **Cypher** in Vegas Strip South and give it to him (e.g., `GIVE RED PILL TO CYPHER`).
            4. Upon receiving the pill, Cypher's demeanor changes. He looks directly at the player and says:  
               *"You’ve made your choice. The truth is out there, hidden in the source of the matrix. Break out of the quarantine, and you’ll find what you seek. But beware-once you see it, there’s no going back."*  
               This cryptic message hints that the player must examine the `.z5` game file itself to uncover the hidden room.
    * **Phase 3: Decoding the Flag**
        *   **Action:** Players must decrypt the extracted encoded flag (`0x2b2d35333f3c3414262523763d3a7778340c29`) using the provided hints.
        *   **Hints:**
            1. The flag starts with `flag{`.
            2. The key to decoding the flag is **5 bytes long** hence it is a repeating XOR cipher with a 5 byte key.
            3. key = C[0..4] XOR b"flag{" = 0x4d 0x41 0x54 0x54 0x44 = "MATTD"
        *   **Description:**
            1. Players analyze the encoded flag and determine the decoding method using the hints.
            2. The correct decoding process reveals the full flag.
    * **Flag:** `flag{qu@ran7in35uX}`
    * **CTF Points (Hard):** 83
    * **CTFd_Challenge_Name:** `Qu4r4n71n3`

* Phase 1 - 06 Ramen House Menu (Observation, File Repair, Steganography, Physical Puzzle/Overlay)
    * **Overall:** Player finds a chalkboard menu in Japanese, finds a translation book, deciphers a code, uses it on the physical 5n4ck3y to get a corrupted PNG. After repairing it, steganography reveals a cutout. Overlaying the cutout on the repaired menu image and reading in a specific order reveals the flag.
    * **Phase 1: The Encrypted Menu & The Key (In-Game BENDER - Puzzle, Observation)**
        *   **Location:** "Chalkboard" (in "Ramen Noodle House"). "Japanese-English Translation Book" (in "Pawn Shop").
        *   **Description:**
            1.  Player finds a "Chalkboard" in the "Ramen Noodle House". The header says "Menu," but the contents are written in Japanese and are initially unreadable.
            2.  Player explores and finds a "Japanese-English Translation Book" in the "Pawn Shop".
            3.  With the "Japanese-English Translation Book" in inventory, player examines the "Chalkboard" again. The menu is now readable (translated) and reveals a faintly written code: `U25hayBDb2RlLi4uMTMzNy1lZ2dwbGFudC1lZ2dwbGFudC1lZ2dwbGFudA==` 
            4.  Base 64 decodes to: `Snak Code...1337-eggplant-eggplant-eggplant`.
        *   **Key Objects Required:** "Japanese-English Translation Book".
        *   **Output/Hint:** 5n4ck3y Code: `1337-eggplant-eggplant-eggplant`.
    * **Phase 2: Digital Layers & Hidden Messages (IRL Vending Machine, File Repair, Steganography, Overlay Puzzle)**
        *   **Action:** Player inputs the code from Phase 1 (`1337-eggplant-eggplant-eggplant`) into 5n4ck3y (the physical vending machine).
        *   **Description:**
            1.  5n4ck3y "prints" an NFT (e.g., a slip of paper with a QR code to URL https://www.dropbox.com/scl/fi/h7rzvnaa18ae2rluq7jrm/MENUS.png?rlkey=yoycej835u9i7jkkk3qjzdmxp&st=xx0yxz8w&dl=0).
            2.  The NFT links to a Dropbox URL where the player can download a corrupted PNG file (e.g., `menu.png`).
            3.  Player must inspect the file with a hex editor and repair the PNG header to make it viewable. The repaired image appears to be a normal ramen menu.
            4.  The repaired menu image contains a subtle hint about a "secret menu" or that "the REAL flavor is hidden deeper in what you see" suggesting steganography.
            5.  Player uses a steganography tool (e.g., `steghide`, `zsteg`, or an online tool) on the repaired image to extract a hidden file.
            6.  The extracted file is another PNG (e.g., `menu2.png`). This image is mostly black with transparent sections, acting as a cutout template.
            7.  Player must overlay the `menu2.png` (digitally, or by printing and physically overlaying) onto the `menu.png`.
            8.  The overlay will reveal a secret menu with transparent cutouts. Overlaying on the first menu words appear. They have Japanese Katakana detailing Flag, First (Hacker), Second(Miso), Third(Seaweed), and Fourth(Sake).
        *   **Key Objects Required:** Hex editor, steganography tool. (The translation book is for Phase 1)
        *   **Output/Hint:** The flag.
        *   **5N4CK3Y INTERACTION:** 5n4ck3y dispenses the NFT link after the correct code is entered.
    * **Flag:** `flag{HackerMisoSeaweedSake}`
    * **CTF Points (Medium):** 71
    * **CTFd_Challenge_Name:** `H4x0r_D3lux3`

 * Phase 1 - 07 NPC-crets (Distributed Puzzle, NPC Interaction)
    * **Overall:** Player engages in a fetch-quest chain across four NPCs, obtaining items for each in sequence. The fifth NPC (Oracle, the bartender) provides the initial item that allows the player to start fulfilling the requests back up the chain. Each of the first four NPCs, upon receiving their desired item, gives the player an encoded word. The player must decode these four words and assemble them into a passphrase, which is then told to Oracle to receive a 5n4ck3y NFT PIN Code.
    * **Phase 1: The Favor Chain & Fragment Collection (Exploration, NPC Interaction, Item Quest)**
        *   **NPCs & Locations:**
            *   **NPC 1: "Scanline" the Analog Archivist** - Found in `BlockbusterVideo`.
            *   **NPC 2: "Proxy" the Information Broker** - Found in `SewerMazeTM10`.
            *   **NPC 3: "Diva" the Cybernetic Showgirl** - Found in `VegasStripSouthCentral`.
            *   **NPC 4: "Alleyway Al" the Paranoid Hobo** - Found in `VegasStripNorth`.
            *   **NPC 5: "Oracle" the Bartender** - Found in `RivieraBarAndLounge`.
        *   **The Fetch Quest Details (Player Initiates by Talking to Scanline):**
            1.  Player talks to **Scanline (NPC1)**. Scanline says he has some good info but first needs **"An Obscure Technical Manual for a Vintage Synthesizer"** which he believes Proxy (NPC2) can unearth.
            2.  Player talks to **Proxy (NPC2)**. Proxy is willing to find the manual, but only if the player can secure **"An Encrypted Data Chip from a Rival Casino's High Roller Suite"** from **Diva (NPC3)**, for its valuable intel.
            3.  Player talks to **Diva (NPC3)**. Diva has access to such a chip but needs **"A collection of rare, vintage neon tube filaments"** from **Alleyway Al (NPC4)** for a stunning new costume piece.
            4.  Player talks to **Alleyway Al (NPC4)**. Alleyway Al will provide the neon filaments but first requires his favorite drink: the "Zero-Day Cocktail," which only Oracle (NPC5) at the Riviera Bar can make perfectly.
            5.  Player talks to **Oracle (NPC5)**. Oracle, with a knowing smile, agrees to make the "Zero-Day Cocktail" for Alleyway Al, on the house. This gives the player the first item to start reversing the chain of favors.
        *   **Delivering Items & Receiving Encoded Words (Working Backwards):**
            1.  Player gives the "Zero-Day Cocktail" to **Alleyway Al (NPC4)**. Alleyway Al is delighted, hands over the "Collection of rare, vintage neon tube filaments," and provides the first encoded word.
            2.  Player gives the "Collection of rare, vintage neon tube filaments" to **Diva (NPC3)**. Diva is thrilled with the find, gives "An Encrypted Data Chip from a Rival Casino's High Roller Suite," and offers the second encoded word.
            3.  Player gives "An Encrypted Data Chip from a Rival Casino's High Roller Suite" to **Proxy (NPC2)**. Proxy nods approvingly, provides "An Obscure Technical Manual for a Vintage Synthesizer," and mutters the third encoded word.
            4.  Player gives "An Obscure Technical Manual for a Vintage Synthesizer" to **Scanline (NPC1)**. Scanline is thrilled and shares the fourth and final encoded word.
        *   **Encoded Words and Decryption Clues (Order of acquisition by player):**
            *   **From Alleyway Al (NPC4) - Word 1 (Medium Difficulty):**
                *   Decodes to: "TELL"
                *   Encoded: `BAABAAABAAABABAABABA`
                *   Cipher: Baconian Cipher (e.g., A=0, B=1 or distinct styles; T=BAABA, E=AABAA, L=ABABA)
                *   Hint from Alleyway Al: "That cocktail... it hit the spot! Would go great with a side of bacon, you know? My info for ya? It's all black and white, like old TV static, or maybe... two distinct choices for every letter. You gotta see the pattern in the noise."
            *   **From Diva (NPC3) - Word 2 (Medium-Hard Difficulty):**
                *   Decodes to: "ORACLE"
                *   Encoded: `RZVCOM`
                *   Cipher: Vigenère Cipher (Keyword: `DIVA`)
                *   Hint from Diva: "These filaments will make my new costume *shine*! My little secret for you, darling? It's like a dance, a repeating pattern. My name sets the rhythm, shifting each letter just so. You'll need to know the steps."
            *   **From Proxy (NPC2) - Word 3 (Hard Difficulty):**
                *   Decodes to: "ABOUT"
                *   Encoded: `BYTCA`
                *   Cipher: Bifid Cipher (Keyword: `PROXY`)
                *   Hint from Proxy: "This data chip... valuable. My information for you is encoded in pairs, split and merged. My callsign is the key to the grid."
            *   **From Scanline (NPC1) - Word 4 (Multi-layered encoding):**
                *   Decodes to: "MATTDAMON"
                *   Encoded: `NEU1QTQ3NDc1NzVBNEU0QzRE`
                *   Cipher: Multi-Layered Obfuscation:
                    1. **Atbash Cipher**: `MATTDAMON` -> `NZGGWZNLM`
                    2. **Hexadecimal Encoding**: `NZGGWZNLM` -> `4E5A4747575A4E4C4D`
                    3. **Base64 Encoding**: `4E5A4747575A4E4C4D` -> `NEU1QTQ3NDc1NzVBNEU0QzRE`
                *   Hint from Scanline: "This synthesizer manual, a true classic for the archives! My final piece of the puzzle is buried under three distinct layers-peel them back one by one, and the melody will reveal itself."
    * **Phase 2: Assembling the Passphrase & Reporting to Oracle (Decoding, NPC Interaction)**
        *   **Description:**
            1.  Player decodes the four words using the hints provided by each NPC:
                *   `BAABAAABAAABABAABABA` (Baconian) -> "TELL" (from Alleyway Al)
                *   `RZVCOM` (Vigenère with key `DIVA`) -> "ORACLE" (from Diva)
                *   `BYTCA` (Bifid with key `PROXY`) -> "ABOUT" (from Proxy)
                *   `NEU1QTQ3NDc1NzVBNEU0QzRE` (Multi-layered: Atbash -> Hexadecimal -> Base64) -> "MATTDAMON" (from Scanline)
            2.  Player assembles the decoded words in the order they were obtained from the NPCs (Alleyway Al's word, then Diva's, then Proxy's, then Scanline's) to form the passphrase: "TELL ORACLE ABOUT MATTDAMON".
            3.  Player tells the complete passphrase "TELL ORACLE ABOUT MATTDAMON" to **Oracle (NPC5)** in the `RivieraBarAndLounge`.
            4.  If correct, "Oracle" acknowledges the message and provides the player with a 5n4ck3y NFT PIN Code, which is Base64 encoded: `TUFUVF9CVVRUT04tTUFUVF9CVVRUT04tQjNGNEMzLUVnZ3BsYW50LUVnZ3BsYW50`.
        *   **Key Objects Required:** The conceptual items for the fetch quest ("Zero-Day Cocktail", "Collection of rare, vintage neon tube filaments", "An Encrypted Data Chip from a Rival Casino's High Roller Suite", "An Obscure Technical Manual for a Vintage Synthesizer"). These would likely be represented by flags or temporary player inventory items in the game logic.
        *   **Output/Hint:** A Base64 encoded 5n4ck3y NFT PIN Code from "Oracle". Player will need to decode this to get `MATT_BUTTON-MATT_BUTTON-B3F4C3-Eggplant-Eggplant`.
        *   **5N4CK3Y INTERACTION:** Player decodes the Base64 string `TUFUVF9CVVRUT04tTUFUVF9CVVRUT04tQjNGNEMzLUVnZ3BsYW50LUVnZ3BsYW50` obtained from "Oracle" to get `MATT_BUTTON-MATT_BUTTON-B3F4C3-Eggplant-Eggplant`. They then type this decoded PIN into 5n4ck3y to obtain the flag.
    * **Flag:** `flag{0N3_5M411_F4V0R_PLZ}` (This will be obtained after using the decoded NFT PIN code with 5n4ck3y)
    * **CTF Points (Hard):** 78
    * **CTFd_Challenge_Name:** `NPC_cr37z`

* Phase 1 - 08 Dune Pinball (Game Interaction)
    * **Overall:** Player interacts with a Dune-themed pinball machine machine IRL (yes we are bringing pinball machines). The machine features references to the movie *Dune* and provides a ticket upon successful interaction, which can be redeemed for the flag.
    * **Phase 1: The Spice Must Flow (Game Interaction)**
        *   **Location:** HackRCade.
        *   **Description:**
            1.  Player finds a pinball machine titled "Dune: The Summoning of Shai-Hulud" in the HackRCade.
                *   *Pinball Machine Description:* "The machine is an intricate homage to the movie *Dune*. Its backglass features a stunning depiction of Paul Atreides standing on the sands of Arrakis, with a massive sandworm rising behind him. The playfield is a desert landscape, complete with miniature spice harvesters, Fremen warriors, and a rotating sandworm that looms ominously. The machine hums with a haunting melody, and the display flashes: 'Summon Shai-Hulud and sacrifice the ball to the sands! A ticket will flag you as the true Mahdi'"
            2.  Interacting with the machine (e.g., `PLAY PINBALL`) starts the game.
            3.  The objective is to activate the thumper by hitting specific targets on the playfield. Once the thumper is activated, the sandworm (Shai-Hulud) begins to rise.
            4.  The player must then maneuver the ball into the sandworm's mouth, symbolizing a sacrifice to the sands of Arrakis. If successful, the machine lights up and dispenses a ticket.
            5.  The player can redeem the ticket at the counter in the HackRCade to receive the flag.
        *   **Key Objects Required:** None.
        *   **Output/Hint:** The ticket is dispensed by the pinball machine upon success. Redeeming the ticket at the counter provides the flag.
    * **CTFd_Challenge_Name:** `Dun3_P1nb4ll_Ch4ll3ng3`
    * **Flag:** `flag{Th3_5p1c3_Mu57_Fl0w}`
    * **CTF Points (Hard):** 76

* Phase 1 - 09 Labyrinth Pinball (Game Interaction)
    * **Overall:** Player interacts with a Labyrinth-themed pinball machine IRL (yes we are bringing pinball machines). The machine features references to the movie *Labyrinth* and provides the flag upon successful interaction.
    * **Phase 1: Navigating the Maze (Game Interaction)**
        *   **Location:** HackRCade.
        *   **Description:**
            1.  Player finds a pinball machine titled "Labyrinth: The Goblin King's Challenge" in the HackRCade.
                *   *Pinball Machine Description:* "The machine is a dazzling tribute to the movie *Labyrinth*. Its backglass features a striking image of Jareth, the Goblin King, holding a crystal ball, with Sarah and the labyrinth in the background. The playfield is a chaotic maze of ramps, spinners, and targets, adorned with miniature goblins, crystal orbs, and a tiny replica of the Escher-like staircase room. The machine hums with an eerie tune, and the display flashes: 'Do you dare to challenge the Goblin King? You probably want to H E L P instead...'"
                *   TLDR Light up "H E L P" on the playfield inlanes and outlanes to get the flag.
            2.  Interacting with the machine IRL starts the game.
            3.  Upon successful interaction, the machine displays the flag on its screen: `flag{Y0uH4v3N0P0w3r0v3rM3}`.
        *   **Key Objects Required:** None.
        *   **Output/Hint:** The flag is displayed on the pinball machine's screen after interaction.
    * **CTFd_Challenge_Name:** `L4byr1n7h_P1nb4ll_Ch4ll3ng3`
    * **Flag:** `flag{Y0uH4v3N0P0w3r0v3rM3}`
    * **CTF Points (Hard):** 74

* Phase 1 - 10 Cr4abfoam Yaarcade Pinball
    * **Overall:** Player interacts with a Cr4b-themed pinball machine in Hack Arcade, it is offline and has a message saying look at DEFCON meshtastic, social media, and CTFd pop ups for details.
    * **Phase 1: Navigating the Maze (Game Interaction)**
        *   **Location:** IRL Location Treasure Island Arcade. Nothing IN Game other than a dead machine in HackArcade.
        *   **Description:**
            1.  When we have confirmed date place and time we will send a CTFd notification to all players.
            2.  YARRR! Welcome to The Cove - where neon tides meet steel balls and only the saltiest dogs dare to duel Crabfoam, keeper of tilt and terror. Step up to the pinball plank at Treasure Island Arcade. If ye dare flip flippers with Crabfoam and live to tell the tale, you earn the flag. But beat 'em, and a special badge shall be bestowed upon ye, forged from bits, brawn, and brine. Location: The Cove Bar & Arcade Requirement: Play a full game against Crabfoam. Bonus: Beat Crabfoam’s score to claim the cursed badge.
        *   **Key Objects Required:** None.
        *   **Output/Hint:** We give people a flag for competing and just hand them a badge if they beat Cr4bf0am.
    * **CTFd_Challenge_Name:** `Y4RRc4D3_Du3L_P1nb4ll_Ch4ll3ng3`
    * **Flag:** `flag{walked_the_plank_or_flipped_the_foam}`
    * **CTF Points (Medium):** 38
    * **CTFd_Challenge_Name:** `Y4RRc4D3_Du3L_P1nb4ll_Ch@mp!0n`
    * **Flag:** `flag{walked_the_plank_and_beat_the_foam}`
    * **CTF Points (Hard):** 99

* Phase 1 - 11 5n4ck3y HAL's Eye (Morse & Polybius Cipher)
    * **Overall:** Player finds in-game clues on a PDA (5n4ck3y code and a hint about HAL). This points to a maintenance panel in the sewer (SewerMazeTM09). Upon opening the panel, they find a message written on its interior revealing the Polybius keyword and construction details. 5n4ck3y's HAL-like light blinks Morse code for number pairs (coordinates). These are used with the Polybius Square to decrypt a core message, which is then formatted into the flag.
    * **Phase 1: The Initial Vector & The Ancient Square's Key (In-Game BENDER)**
        *   **Locations:**
            *   "Discarded Technician's PDA" (in "Riviera Casino Floor").
            *   "Maintenance Panel" (in "SewerMazeTM09").
        *   **Description:**
            1.  Player finds and examines the "Discarded Technician's PDA" in the "Riviera Casino Floor".
            2.  The PDA displays:
                *   A hint: "Slot Technician HAL has one eye, but speaks in pairs."
                *   A pointer: "Ciphertext Location: ERROR Under Maintenance...Panel?" (This now primarily serves to guide the player to the panel).
                *   "He also tracks the maintenance override code for the vending machine in a strange way:
                     - 3957 has 1 digit correct: in the wrong position.
                     - 2148 has 3 digits correct: 1 in correct position, 2 in wrong positions.
                     - 6254 has 3 digits correct: all in the wrong positions.
                     - 4031 has 2 digits correct: 1 in correct position, 1 in wrong position.
                *   **Note:** The correct code is 4125. From the clue "3957 has 1 digit correct in the wrong position," we know only one of those digits is in the code, and it's misplaced. "2148 has 3 digits correct: 1 in the correct position, 2 in the wrong positions" suggests 1, 2, and 4 are all in the code. "6254 has 3 correct digits, all misplaced," confirming 2, 5, and 4 are in the code but in different spots. Finally, "4031 has 2 correct digits: 1 correct place, 1 wrong place" helps lock down positions. Combining all clues leads uniquely to 4125.
            3.  Player navigates to "SewerMazeTM09".
            4.  Player finds and examines the "Maintenance Panel" (e.g., `SewerPanelTM9`). The exterior might appear normal or show signs of tampering.
            5.  Upon opening or further examining the interior of the panel, the player finds a message scrawled in faded marker:
                "The KEY to pissing me off is this damn summer HACKERCON. Fuckers put stickers everywhere and break everything! Its bad enough I only have one eye, but the fact that I cant tell the difference between the letters I and J makes it even worse. I'm gonna quit one day... Sincerely, Hal Polybius"
                (This message provides the Polybius keyword `DEFCON` and the rule to combine `I` and `J`. The player is expected to use a standard 5x5 Polybius construction: fill with keyword then remaining alphabet.)
        *   **Key Objects Required:** "Discarded Technician's PDA" (interaction needed), "Maintenance Panel" in SewerMazeTM09 (interaction needed to find the message inside).
        *   **Output/Hint:** 5n4ck3y Code (e.g., `4125` from PDA). From the message inside the panel: Polybius Square keyword (`DEFCON`) and the I=J construction rule.
    * **Phase 2: HAL's Blinking Coordinates (IRL Vending Machine - Morse Code)**
        *   **Action:** Player inputs the 4-digit code from Phase 1 into the physical 5n4ck3y keypad.
        *   **Description:**
            1.  5n4ck3y's large red HAL-like light blinks a sequence in Morse code. The Morse code translates to a series of number pairs.
            2.  Player records the Morse code sequence.
        *   **Key Objects Required:** Ability to see/record 5n4ck3y's light, knowledge of Morse code for numbers. 
        *   **Output/Hint:** A sequence of 15 number pairs, for example: `[25 22 33 43 12 12 55 22 33 33 35 12 22 42 55]` (This example decodes to `HALSEEZALLPEARZ` using the HACKERCON grid).
        *   **5N4CK3Y INTERACTION:** 5n4ck3y keypad for code entry, HAL light blinks Morse code.
    * **Phase 3: The Garbled Message (IRL Observation)**
        *   **Location:** Physical 5n4ck3y vending machine.
        *   **Description:**
            1.  This phase now represents the player understanding that the blinking lights from HAL's eye (Phase 2) *are* the "garbled message" in numerical form, which they need to decode. The "secret" is what HAL's eye is directly revealing through the number pairs.
        *   **Key Objects Required:** Observation skills (from Phase 2).
    * **Phase 4: Decryption & Victory (Offline/Tool-Assisted)**
        *   **Action:** Player constructs the custom Polybius Square using the keyword and method derived from the message inside the panel (Phase 1). Player uses the number pairs from Phase 2 as row/column coordinates on this square to decrypt the core message.
        *   **Description:** Each number pair from the Morse code maps to a letter on the custom Polybius Square. Decrypting all pairs reveals the core message (e.g., `HALSEEZALLPEARZ`). The player then formats this core message into the final flag string by adding `flag{` and `}`.
        *   **Key Objects Required:** Understanding of Polybius Square, the custom key and construction method (from the message inside the panel in Phase 1), the coordinates (from Phase 2).
        *   **Output/Hint:** The flag.
    * **Flag:** `flag{HALSEEZALLPEARZ}`
    * **CTF Points (Medium):** 61
    * **CTFd_Challenge_Name:** `H4l5_R3d_3y3d_Publ1c_D15pl4y_0f_Affl1c710n`

* Phase 1 - 12 Red Box Rhapsody (Phone Phreaking, Audio Analysis, ASCII)
    * **Overall:** A multi-phase challenge involving an in-game phone phreaking puzzle to get a code for a physical vending machine. The machine plays a sequence of payphone coin tones which, when decoded and combined with an in-game hint, reveal ASCII characters forming the flag.
    * **Phase 1: The Operator's Code (In-Game BENDER - Puzzle, Observation)**
        *   **Location:** "Payphone" object (in "Seedy Club") AND "Discarded Electronics Pile" (in "Sketchy Alleyway").
        *   **Description:**
            1.  Player examines a "Payphone". Scratched text hints: "The year the best phreakin quarterly journal for operators hit the stand. J.E. and 5n4ck3y would appreciate this year. It was a good year." (Leads to code 1984).
            2.  Player searches a "Discarded Electronics Pile" and finds a "Cracked Blue Box Casing."
            3.  Examining the "Cracked Blue Box Casing" reveals a label: "Property of J.E. - Tones speak numbers, numbers sing songs. For this special snack service, the numbers sing in plain **ASCII**."
        *   **Key Objects Required:** None to take, but interaction with "Payphone" and "Cracked Blue Box Casing" are needed.
        *   **Output/Hint:** 5n4ck3y Code: `1984`, Hint: "The numbers sing in plain ASCII."
    * **Phase 2: Tones of Treasure (IRL Vending Machine - Audio Analysis)**
        *   **Action:** Player inputs the code from Phase 1 (`1984`) into 5n4ck3y (the physical vending machine).
        *   **Description:** The vending machine plays a sequence of 10 tone sets in WAV format, each containing tones representing a monetary value. Player must record and decode these coin tones.
        *   **WAVs (for flag AUx_#_snAx):** Tones for $0.65 (A), $0.85 (U), $1.20 (x), $0.95 (_), $0.35 (#), $0.95 (_), $1.15 (s), $1.10 (n), $0.65 (A), $1.20 (x).
    * **Phase 3: Assembling the Call (Interpretation, ASCII Conversion)**
        *   **Action:** Combine decoded monetary values with the hint from Phase 1.
        *   **Description:** Convert each decoded monetary value (in cents) into its corresponding ASCII character using the hint "The numbers sing in plain ASCII."
        *   **Solution Example:** $0.65 -> 65 cents -> ASCII 65 -> 'A'. Repeat for all 10 values.
    * **Flag:** `flag{AUx_#_snAx}`
    * **CTF Points (Medium):** 57
    * **CTFd_Challenge_Name:** `R3d_B0x_Rh4p50dy`
  
* Phase 1 - 13 Collector
    * **Overall:** Player collects 5 casino chip objects scattered throughout the game. Each chip contains a slice of an encoded message. Once all chips are collected, they can be exchanged with an NPC at the Riviera Casino floor for a decoder key/method. This is then used to decrypt the combined message slices to reveal the flag.
    * **Phase 1: Collecting the Chips & Getting the Key (In-Game BENDER - Exploration, Collection, NPC Interaction)**
        *   **Location:**
            *   Casino Chip #1: `SewerMazeTM03` (Inside a "Discarded, Waterlogged Boot" - new scenery)
            *   Casino Chip #2: `SewerMazeTM06` (Inside a "Discarded Rusty Can" in the equipment pile - new scenery)
            *   Casino Chip #3: `BlockbusterVideo` (Inside a "VHS Tape Case" for a specific movie - new object)
            *   Casino Chip #4: `RivieraHotelLobby` (e.g., Inside a "Plush Lobby Armchair" which has a Tear in the Upholstery - new scenery/object)
            *   Casino Chip #5: `SewerMazeTM12` (e.g., Tucked into a "Crack in the Wall" - new scenery)
            *   Chip Exchange NPC/Counter: "RivieraCasinoFloor"
        *   **Description:**
            1.  Player explores the game world to find and take 5 distinct casino chip objects. Chips are hidden within thematic objects or scenery in their respective locations, requiring actions like 'search', 'look in', 'open', or 'examine' on the concealing item.
            2.  The description of each chip contains its number and a segment of an encoded message in hexadecimal. The full, ordered hexadecimal ciphertext to be assembled is `4465636f6465204d653a205c6b5e5c6b3454276f4e6a215c5f5550725e535c4b39676665636e`. The slices (prefixed with "0x" for clarity on the chip) are:
                *   Chip #1: "Slice 1/5: 0x4465636f6465204d"
                *   Chip #2: "Slice 2/5: 0x653a205c6b5e5c6b"
                *   Chip #3: "Slice 3/5: 0x3454276f4e6a215c"
                *   Chip #4: "Slice 4/5: 0x5f5550725e535c"
                *   Chip #5: "Slice 5/5: 0x4b39676665636e"
            3.  Each chip's description also hints: "Collect all 5 unique chips and bring them to the exchange counter at the Riviera Casino floor."
            4.  Once the player has all 5 casino chips in their inventory, they must go to the "RivieraCasinoFloor".
            5.  Player interacts with a "Chip Exchange Clerk" NPC or a "Chip Exchange Counter" object.
            6.  Upon successful interaction (verifying all 5 unique chips are present), the NPC takes the chips and provides the player with an IRL 5n4ck3y vend code: `D3C0D3DC0D35`.  Once the vend code is typed in to the keypad, a hint with the decoder keyword `JACKPOT` will be supplied to the player.
        *   **Key Objects Required:** The 5 specific casino chip objects.
        *   **Output/Hint:** The decryption method (Keyword `JACKPOT` and ASCII-based shift rules). Each chip provides a hint to collect all 5 and visit the Riviera.
    * **Phase 2: Assembling and Decoding the Message (Puzzle, Cryptography)**
        *   **Action:** Player combines the encoded message slices and uses the provided decryption method.
        *   **Description:**
            1.  Player assembles the 5 encoded hexadecimal message slices (e.g., "0x4465636f6465204d", "0x653a205c6b5e5c6b", etc.) from the chip descriptions in the correct order. They will need to concatenate the hexadecimal values (removing the "0x" prefixes for processing if their tool requires it) to form the complete ciphertext: `4465636f6465204d653a205c6b5e5c6b3454276f4e6a215c5f5550725e535c4b39676665636e`.
            2.  Player first converts this hexadecimal string back to its ASCII representation. The result will be: "Decode Me: \k^\k4T'oNj!\_UPr^S\K9gfecn".
            3.  Player then takes the substring *after* "Decode Me: " (which is `\k^\k4T'oNj!\_UPr^S\K9gfecn`) and reverses the encryption on this substring using the keyword `JACKPOT` and the rules provided by the QR Code Hint:
                *   For each character in this substring, determine the alphabetical position of the corresponding key character (A=1, ..., Z=26).
                *   Shift the encoded character's ASCII value *forward* by this alphabetical position.
                *   If the new ASCII value goes above 126, subtract 95 to wrap around into the printable range (ASCII 32-126).
                *   Actual text string from QR Code Printout: 
                        "Take a seat at the ASCII table and you're sure to hit a JACKPOT"
            4.  Successful decryption of the substring will yield the original plaintext: `flag{Ch1pQu1ks_Quick_Chips}`.
        *   **Key Objects Required:** The decryption method (Keyword `JACKPOT` and rules) obtained in Phase 1. The 5 message slices from the chip objects.
        *   **Output/Hint:** The flag.
        *   **5N4CK3Y INTERACTION:** Type in vend code `D3C0D3DC0D35` obtained from casino clerk to obtain decryption keyword & hint.
    * **Flag:** `flag{Ch1pQu1ks_Quick_Chips}`
    * **CTF Points (Medium):** 66
    * **CTFd_Challenge_Name:** `C0ll3c73r5_r_ju57_H04rd3r5_w17h_a_h0bby`

 * Phase 1 - 14 Remote Revelation (IR Puzzle)
    * **Overall:** Player finds clues in-game to determine a 5n4ck3y keypad code and a specific TV remote IR code. Entering the keypad code primes 5n4ck3y to listen for an IR signal. Sending the correct TV IR code results in 5n4ck3y dispensing the flag.
    * **Phase 1: Piecing Together the Signal (In-Game BENDER - Puzzle, Observation)**
        *   **Locations:**
            *   "Battered Universal Remote" (in "Tattoo Parlor").
            *   "Conference Room A/V Notes" (in "Riviera Hotel Conference Room").
        *   **Description:**
            1.  Player finds and examines the "Battered Universal Remote" in the "Tattoo Parlor".
                *   Its description reveals: "This poor universal remote has seen rougher days than a dial-up modem^
                     during a lightning storm. It's sticky, probably from a close encounter^
                     with a Jolt Cola. It has a prominent button clearly labeled 'INPUT'. Other buttons for 'TV', 'VIDEO', 'HDMI1', 'HDMI2', and 'AUX' are also present, though some look like they've been rage-quit^
                     one too many times. Embossed faintly on the plastic is 'Property of the Riviera Hotel'.^
                     Who would steal a remote from the hotel, especially one in this condition?^
                     A faded label on the back, barely clinging to life, reads: 'SONY TV - Conf. Room Setup - DO NOT REMOVE, HAL!'.^
                     Taped haphazardly over the empty battery compartment is a grimy, yellowed
                     sticky note. In faded, almost ghostly ink, it proclaims:
                     '5n4ck3y IR ACTIVATE CODE: B33F33'
            2.  Player finds and examines the "Conference Room A/V Notes" in the "Riviera Hotel Conference Room".
                *   Its description reveals: "A crumpled sheet of paper titled 'Conference Room A/V - SONY Bravia Quick Setup'. Most of it is coffee-stained. One legible section details TV specs: 'SONY Bravia - System Code: 01. Carrier Freq: 38kHz (standard)'. Another clear section reads:^
                    'Technician's Remote Code Sequence (S.O.P.):^
                    - Start with the carrier frequency of the remote in kHz.
                    - Multiply this frequency by the numerical value of the System Code.
                    - Add the number of *letters* on the primary 'INPUT' button on the remote.
                    - Take the result MOD 50.^
                    Log this final value as the active IR code. DO NOT LOSE THIS. AGAIN.'"
                    *(Puzzle solution: (38 * 1 + 5) MOD 50 = 43)*
        *   **Key Objects Required:** None to take, but interaction with both the "Battered Universal Remote" and "Conference Room A/V Notes" is necessary.
        *   **Output/Hint:** 5n4ck3y Keypad Code (e.g., `B33F33`), TV Brand (SONY), Specific TV IR Code (derived as `43` using the System Code and the count of letters on the 'INPUT' button).
    * **Phase 2: Broadcasting the Code (IRL Vending Machine - IR Interaction)**
        *   **Action:** Player inputs the 4-digit hex code (e.g., `B33F33`) from Phase 1 into the physical 5n4ck3y keypad.
        *   **Description:**
            1.  Upon entering the correct keypad code,  5n4ck3y indicates it is now listening for an IR signal (e.g., a message on its screen).
            2.  Player uses a physical universal TV remote, a phone with IR capabilities, or a device like a Flipper Zero to transmit the specific SONY TV IR code (`0x2B`, Decimal 43) to 5n4ck3y's IR receiver.
            3.  If the correct IR code is received, 5n4ck3y dispenses the flag (e.g., prints an NFT with the flag, displays it on its screen, or vends a physical item with the flag).
            4.  We specifically used a Sony Bravia TV code of this year because its a non standard code that is not in the IR database of most devices or TV B Gone zapper devices wont use. Player would have to program their own Arduino device or Flipper Zero to send this specific IR code.
        *   **Key Objects Required:** A device capable of transmitting TV IR codes.
        *   **Output/Hint:** The flag.
        *   **5N4CK3Y INTERACTION:** Player enters a code on 5n4ck3y's keypad. 5n4ck3y then receives an IR signal.
    * **Flag:** `flag{m46n37b0xp4n4ph0n1c50rny}`
    * **CTF Points (Medium):** 51
    * **CTFd_Challenge_Name:** `R3m073_R3v3l4710n`

 * Phase 2 - 01 Badge Hardware Hacking Hidden UART
    * **Overall:** Player discovers and interacts with a hidden UART on the badge hardware to obtain a flag and potentially hints for other hardware challenges. The in-game BENDER puzzle involves finding and using objects to unlock the clue about the badge's UART interface and communication rate.
    * **Phase 1: Locust Decoder Puzzle (In-Game BENDER - Exploration, Item Use)**
        *   **Location:** Dead End Dumpsters Trash (Locust Decoder) and LVCC Loop North Entrance (Locust Shrine).
        *   **Description:**
            1.  Player explores the Dead End Dumpsters Trash area and finds the **Locust Decoder** object hidden in the trash. Its description hints at its connection to locusts and DEF CON 28.
                *   *Locust Decoder Description:* "Amidst the discarded vape pens and shattered AR glasses, you uncover a small, battered device shaped like a locust. Its antennae are bent, and faint etchings on its surface read DEF CON 28. The device hums faintly, as if waiting to be activated."
            2.  Player takes the **Locust Decoder** and brings it to the **Locust Shrine** located at the LVCC Loop North Entrance.
                *   *Locust Shrine Description:* "A strange altar covered in locust motifs. The antennae of the locust carvings seem to point toward something unseen. A faint inscription reads: The whispers of the locust reveal the path."
            3.  Player uses the **Locust Decoder** on the **Locust Shrine** (e.g., `USE LOCUST DECODER ON SHRINE`).
            4.  Upon successful interaction, the shrine activates and provides the clue about the badge's UART interface and communication rate:
                *   *Clue:* "The locust's whispers guide you. Their antennae transmit and receive locust talk. Connect to them to understand their invasion plans and listen carefully-their rate of talk is slow yet leet."
        *   **Key Objects Required (New In-Game):** Locust Decoder (found in Dead End Dumpsters Trash).
        *   **Output/Hint:** The clue about the badge's UART interface and communication rate (31337 baud).
    * **Phase 2: UART Discovery & Interaction (Hardware Hacking, Serial Communication)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1.  Following the in-game hint, player identifies the UART pins on the badge PCB.
            2.  Player uses appropriate hardware tools (e.g., USB-to-Serial adapter, jumper wires) to connect to the UART pins.
            3.  Player uses a terminal emulator (e.g., minicom, PuTTY) to connect to the serial port, determining the correct communication rate, which is 31337 baud.
            4.  Upon successful connection, the UART interface displays a loop with the name of the challenge and flag.
        *   **Key Objects Required:** Soldering iron/probes, USB-to-Serial adapter, terminal software.
        *   **Output/Hint:** A flag or additional hints for other hardware challenges.
    * **Flag:** `flag{UART_Rly_L1sten1ng}`
    * **CTF Points (Hard):** 88
    * **CTFd_Challenge_Name:** `L0cu57_Shr1n3`

 * Phase 2 - 02 Badge Hardware Hacking - Cut a Hidden Trace
    * **Overall:** Player must identify and physically CUT a specific hidden trace on the badge PCB, under one of the battery mounts, to enable a flag displayed on the hidden UART. The in-game BENDER puzzle involves finding and using objects to uncover the hint about the trace.
    * **Phase 1: Energizer Bunny Puzzle (In-Game BENDER - Exploration, Item Use)**
        *   **Location:** EV Charging Station (Energizer Bunny Object) and Vape Shop (Drum Mallet Object).
        *   **Description:**
            1.  Player explores the EV Charging Station and finds the **Energizer Bunny Object**. Its description hints at its connection to power and locusts, and it mentions that the bunny is missing its drum mallet.
                *   *Energizer Bunny Object Description:* "A corroded battery pack shaped like a bunny, its surface strangely crawling with small, metallic-looking locusts. Its drum mallet is missing, and the faint smell of ozone lingers around it. The words Power flows where the bunny beats are etched into its surface. The bunny looks like it hasn’t drummed in years, but maybe it’s waiting for something..."
            2.  Player explores the Vape Shop and finds the **Drum Mallet Object**. Its description hints at its connection to the bunny.
                *   *Drum Mallet Object Description:* "A sturdy percussion mallet with locust carvings etched into its handle. The carvings pulse faintly, as if waiting for something. A faint inscription reads: The bunny beats for those who already speak the locusts' language."
            3.  Player must take the **Drum Mallet Object** and bring it to the **Energizer Bunny Object** in the EV Charging Station.
            4.  Player uses the **Drum Mallet Object** on the **Energizer Bunny Object** (e.g., `USE MALLET ON BUNNY`).
            5.  Upon successful interaction, the game displays the following message, which includes the hint:
                *   *Interaction Output:* "As you use the Drum Mallet on the Energizer Bunny, it transforms into a hacker-modded marvel, strangely covered in a swarm of twitching, metallic locusts! Its eyes glow with menacing red LEDs, and it's belting out a surprisingly clear, synthesized rendition of 'Never Gonna Give You Up, Never Gonna Let You Down' while drumming erratically. This is... unexpected. Amidst the robotic crooning and the chittering of locusts, it continues to whisper intermittently: I am the holder of 1000 mAh, yet I must be removed to cut all traces of my existence. Under the battery holder lies the path to silence. Sever the trace to uncover the truth."
        *   **Key Objects Required:** Drum Mallet Object (found in Vape Shop).
        *   **Output/Hint:** The hint "I am the holder of 1000 mAh, yet I must be removed to cut all traces of my existence. Under the battery holder lies the path to silence. Sever the trace to uncover the truth." (delivered as part of the interaction output).
    * **Phase 2: Trace Identification & Severing (Hardware Hacking, PCB Analysis)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1.  Following the hint from the Energizer Bunny Puzzle, the player inspects the badge PCB and identifies the battery holders.
            2.  Player carefully desolders one of the battery holders on the PCB.
            3.  Beneath the holder, the player identifies a specific, thin trace.
            4.  Player uses a sharp tool (e.g., hobby knife) to carefully cut the specified trace.
            5.  Cutting the trace alters the badge's behavior. If the Hidden UART (Phase 2 - 01) is active, a new flag will now be displayed via the UART serial output.
        *   **Key Objects Required:** Soldering iron (for desoldering battery holders), hobby knife or similar cutting tool. Access to the Hidden UART output.
        *   **Output/Hint:** A flag displayed via the Hidden UART after the trace is cut and the UART challenge is active.
    * **Flag:** `flag{TR4CE_T3RMIN4T3D}`
    * **CTF Points (Medium):** 72
    * **CTFd_Challenge_Name:** `K33p5_G01ng_&_G01ng`

 * Phase 2 - 03 Badge Hardware Hacking - Connect a Hidden Trace
    * **Overall:** Player must identify and physically CONNECT specific hidden pads/trace on the badge PCB, likely found under an AAA "energy cell" holder, to enable a flag. The in-game BENDER puzzle involves finding and using an object to uncover the hint.
    * **Phase 1: Cybernetic LeapFrog Puzzle (In-Game BENDER - Exploration, Item Use)**
        *   **Location:** `VegasStripSouthCentral` (Cybernetic LeapFrog Object, StylizedLilyPads Object) and `Street West` (Precision Jumper Wire Object).
        *   **Description:**
            1.  Player explores `VegasStripSouthCentral`. The room description should now include: "In the distance, the iconic fountains of a Bellagio-esque resort perform their aquatic ballet, their shimmering pools reflecting the neon... Near the edge of one of the grand pools, a peculiar sight: a cybernetic frog sits dejectedly, covered in metallic locusts, a few feet away from a set of what look like stylized lily pads made of conductive material."
            2.  Player finds the **CyberneticLeapFrog Object**.
                *   *CyberneticLeapFrog Object Description:* "A bizarre cybernetic frog, its chassis gleaming with chrome and blinking LEDs, sits motionless by the edge of the grand pool. It's strangely encrusted with small, metallic-looking locusts that twitch occasionally. Its optical sensors, usually a vibrant green, are dull and lifeless. It seems to be looking longingly at a pair of metallic lily pads nearby. A faint, almost inaudible inscription on its side reads: Powerless, I yearn for the pads' embrace, a current's leap to win this race."
            3.  Player finds the **StylizedLilyPads Object**.
                *   *StylizedLilyPads Object Description:* "Two large, flat metallic pads, shaped like stylized lily pads, are embedded in the ground near the pool's edge. They hum with a faint energy, and a noticeable gap separates them. They look like oversized electrical contacts. A tiny inscription etched between them reads: Bridge the gap, let the current flow, and secrets deep the frog will know."
            4.  Player explores `Street West` and finds the **PrecisionJumperWire Object**.
                *   *PrecisionJumperWire Object Description:* "A short, insulated wire with fine, gold-plated alligator clips at each end. It looks like a precision tool for making temporary electronic connections, perhaps salvaged from some high-end diagnostic equipment of a bygone era. Perfect for bridging a small gap in a circuit."
            5.  Player must take the **PrecisionJumperWire Object** and bring it to `VegasStripSouthCentral`.
            6.  Player uses the **PrecisionJumperWire Object** on the **StylizedLilyPads Object** (e.g., `USE JUMPER ON LILYPADS` or `USE JUMPER ON PADS`).
            7.  Upon successful interaction, the game displays the following message, which includes the hint:
                *   *Interaction Output:* "You carefully clip the Precision Jumper Wire across the gap between the Stylized Lily Pads. Sparks fly for a moment! The nearby Cybernetic LeapFrog suddenly whirs to life, its optical sensors flaring to a bright, predatory green! It emits a series of digitized croaks, then a synthesized voice, surprisingly clear, speaks:
                    Ribbit... Connection established! The locusts scatter from my energized field! You seek another path, another bridge to make? Listen closely, data-streamer:
                    Where three small power sources once did reside,
                    Lift their cradle, look deep inside.
                    A broken path, two points forlorn,
                    Join them true, 'til a new signal's born.
                    The current flows, the circuit sings,
                    Another secret the tasty locust breath brings.
                    The frog then returns to zapping the remaining locusts with renewed vigor."
        *   **Key Objects Required:** PrecisionJumperWire Object (found in Street West).
        *   **Output/Hint:** The riddle: "Where three small power sources once did reside, Lift their cradle, look deep inside. A broken path, two points forlorn, Join them true, 'til a new signal's born. The current flows, the circuit sings, Another secret the tasty locust breath brings."
    * **Phase 2: Trace Identification & Connection (Hardware Hacking, PCB Analysis, Soldering)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1.  Following the hint from the Cybernetic LeapFrog Puzzle, the player inspects the badge PCB and identifies an AAA "energy cell" holder (or similar small, triple-cell power source housing).
            2.  Player carefully desolders or removes the "energy cell" holder from the PCB.
            3.  Beneath the holder, the player identifies two exposed pads or a clearly broken trace that are meant to be connected.
            4.  Player uses a soldering iron and solder (or conductive ink/wire) to bridge the gap between the pads or repair the trace, completing the circuit.
            5.  Connecting the trace alters the badge's behavior. If the locusts are already whispering their secrets (Phase 2 - 01 solved), their tasty locust breath will carry another flag to you through the same hidden channel.
        *   **Key Objects Required:** Soldering iron and solder (or conductive ink/wire). Understanding of the locusts' "whispers" (Phase 2-01).
        *   **Output/Hint:** A flag, delivered via the locusts' hidden channel (their 'tasty breath'), once the trace is connected and their whispers (Phase 2-01) have been understood.
    * **Flag:** `flag{BR1DGE_0VER_TR0UBLED_C0PPER}`
    * **CTF Points (Medium):** 70
    * **CTFd_Challenge_Name:** `Cyphr0g_Jump3r`
    * **Errata**: `We learned there was somewhat of a bug in our implementation due to using a non standard Baud Rate of 31337. Some UART adapters and OS, not all, would cut the flag off after 20 charachters. When this happened contestants showed us the partially printed flag and we were able to verify it was correct.`

 * Phase 2 - 04 Badge Hardware Hacking Flash Chip -> Ch1pQu1ks Chip Clips
    * **Overall:** Player experiences an event on the High Roller, leading them to find items in the Dive Bar and Analog Antiques Store. These items are used to extract a clue from an emergency panel, guiding them to dump the badge's flash memory and find a flag.
    * **Phase 1: The Stalled Signal (In-Game BENDER - Exploration, Item Use, Puzzle)**
        *   **Locations:**
            *   `TopOfHighRoller`: The High Roller stopping is the catalyst. An "Emergency Access Panel" (new scenery object) becomes visible/interactive.
            *   `DiveBar`: Player finds a "Damaged Data Cartridge" (new item).
            *   `AnalogAntiquesStore`: Player finds a "Retro Data-Interface Cable" (new item).
        *   **Description:**
            1.  When the player is in `TopOfHighRoller` and the wheel stops (as per existing room description), the room description should also note: "Amidst the flickering emergency lights, a previously unnoticed 'Emergency Access Panel' on the pod wall is now faintly illuminated."
            2.  Player examines the "Emergency Access Panel". Its description: "A small, reinforced panel with a non-standard data port. A tiny LED next to the port blinks amber. Text beneath reads: 'SYSTEM DIAGNOSTIC PORT - REQUIRES ADAPTIVE INTERFACE. LAST LOG ENTRY: ANOMALOUS MEMORY SIGNATURE DETECTED - SOURCE UNKNOWN.'"
            3.  Player explores the `DiveBar`. Near a passed-out patron (or as a scenery item "Discarded Electronics on Sticky Table"), they find a "Damaged Data Cartridge".
                *   *Damaged Data Cartridge Description:* "A rugged data cartridge, its casing cracked along one edge. A barely legible, singed label reads: 'HRLR - SEC_SYS - MEM_DMP... Property of Ch1pQu1k - CRITICAL - DO NOT LOSE - Contains sensitive data backups & firmware signatures.'"
            4.  Player explores the `AnalogAntiquesStore`. Among the vintage tech, they find a "Retro Data-Interface Cable".
                *   *Retro Data-Interface Cable Description:* "A peculiar tangle of shielded wires terminating in a bizarre assortment of archaic and a few surprisingly modern-looking connectors. It looks like a universal adapter for bridging old tech with new, probably cobbled together from spare parts by a desperate technician."
            5.  Player takes the **Damaged Data Cartridge** and the **Retro Data-Interface Cable**.
            6.  Player returns to `TopOfHighRoller` and uses the **Retro Data-Interface Cable** on the "Emergency Access Panel" (e.g., `USE CABLE ON PANEL` or `CONNECT CABLE TO PANEL`). The game responds: "The multi-connector cable clicks into the panel's strange port. The amber LED turns solid. The panel awaits a data source."
            7.  Player then uses the **Damaged Data Cartridge** with the "Emergency Access Panel" (e.g., `INSERT CARTRIDGE IN PANEL` or `USE CARTRIDGE ON PANEL` - assuming the cable is connected).
            8.  Upon successful interaction, the panel's small integrated screen flickers to life: "Attempting data recovery from damaged cartridge... Sector errors detected... Partial success. Recovered log fragment: '...critical system logs corrupted...badge firmware integrity check failed...possible unauthorized memory access. Ch1pQu1k's custom chip clips are the only reliable way to get a clean dump of the main flash. Look for the secret 'txt'... don't let them overwrite it! System integrity compromised.'"
            9.  After successful interaction and hint, the High Roller suddenly lurches back into motion, and a "down" option is enabled for the player to get back down and escape.
        *   **Key Objects Required (New In-Game):** "Damaged Data Cartridge", "Retro Data-Interface Cable".
        *   **Output/Hint:** The hint about needing "Ch1pQu1k's custom chip clips" to dump the badge's main flash and to look for a .txt file named
                "Ch1pQu1k.txt".
    * **Phase 2: Flash Memory Exfiltration (Hardware Hacking, SPI/I2C, Forensics)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1.  Following the in-game hint, player identifies the flash memory chip on the badge PCB.
            2.  Player uses appropriate hardware tools (e.g., SOIC clip - "Ch1pQu1k's Chip Clips", Bus Pirate, Raspberry Pi with flashrom, JTAG/SWD debugger if applicable) to interface with the flash chip.
            3.  Reference: https://hackaday.io/project/164346-andxor-dc27-badge/log/166065-ftdi-2232h-breakout-for-hardware-hacking, or https://riverloopsecurity.com/blog/2020/02/hw-101-spi/
            4.  Player dumps the entire contents of the flash memory to a binary file.
            5.  Player uses forensic tools like `binwalk` or hex editors to analyze the memory dump.
            6.  The flag is located within a file named `Ch1pQu1k.txt` (as hinted) embedded in the firmware dump. (Implementer: ensure this file with the flag is in `/defcon/fs` for provisioning).
        *   **Key Objects Required:** SOIC clip ("Ch1pQu1k's Chip Clips") or other flash interfacing tools like wire probes, `binwalk` or hex editor.
        *   **Output/Hint:** A flag found within `Ch1pQu1k.txt` in the flash dump.
    * **Flag:** `flag{FL45H_FR13D_F1L35}`
    * **CTF Points (Hard):** 85
    * **CTFd_Challenge_Name:** `H1ghR0ll3rC0n`

  * Phase 2 - 05 Badge Hardware Hacking - Hidden Silkscreen
    * **Overall:** A flag is encoded in the Wingdings font, hidden on the PCB silkscreen, under the screen ribbon cable. The in-game portion provides hints to guide the player toward discovering and decoding the silkscreen text.
    * **Phase 1: Discovering the Clues (In-Game Exploration and Observation)**
        *   **Location:** HackRcade (Display Case and Circuit Sid) and Dive Bar (Magnifying Glass).
        *   **Description:**
            1.  In the HackRcade, the player finds a **Display Case** labeled Secrets of the Silkscreen. Examining it provides a clue about silkscreen text:
                *   "The plaque explains: Silkscreen text is often hidden under components, requiring careful disassembly to reveal. Some designers use it to encode messages, jokes, or even secrets."
            2.  The player encounters **Circuit Sid**, an NPC in the HackRcade. Sid provides a vague hint:
                *   "Hey there, hacker! Did you know some badges hide secrets in their silkscreen? The AND!XOR badge is no exception. You might need to look closely at yours-sometimes the best secrets are hidden where you least expect them. Got a magnifying glass?"
            3.  The player explores the Dive Bar and finds a **Magnifying Glass**:
                *   *Description:* "A magnifying glass with a cracked lens, perfect for inspecting tiny details on circuit boards."
                *   Taking the magnifying glass adds it to the player's inventory.
            4.  With the magnifying glass in their inventory, the player can `LOOK AT BADGE` in their inventory to receive additional hints:
                *   Without the magnifying glass: "The badge is sleek and brimming with LEDs and intricate circuitry. It hums faintly with power, its surface etched with cryptic patterns and hacker-themed designs. A small OLED screen displays scrolling text: AND!XOR."
                *   With the magnifying glass: "The badge is sleek and brimming with LEDs and intricate circuitry. As you inspect it closely with the magnifying glass, you notice something faintly visible behind the screen ribbon. It looks like text, but you’ll need to move the ribbon to see it clearly."
    * **Phase 2: Physical Interaction with the Badge (IRL Hardware Hacking)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1.  Following the in-game hints, the player inspects the badge PCB and identifies the screen ribbon cable.
            2.  The player carefully disassembles the badge to move the screen ribbon cable and reveal the silkscreen text.
            3.  The silkscreen text is written in the Wingdings font and reads something like: flag{by73my5h1nim374l455}
            4.  The player transcribes or photographs the Wingdings characters and decodes them using a Wingdings-to-alphanumeric character map.
    * **Key Objects Required:**
        *   Magnifying Glass (found in the Dive Bar).
        *   Wingdings character map (external resource or player knowledge).
    * **Output/Hint:** The flag, once translated from Wingdings.
    * **Flag:** `flag{by73my5h1nim374l455}`
    * **CTF Points (Medium):** 72
    * **CTFd_Challenge_Name:** `S1lkySm00thD1ng4l1ng`

  * Phase 2 - 06 Badge Hardware Hacking - LED Poop Shoot (Reverse Engineering Hidden Protocols tunneling as NeoPixel WS2812 LED messages)
    * **Overall:** A hidden pin above the poster on the badge is the output from the last LED in the chain. The badge is automatically sending extra LEDs worth of data to this pin (WS2812), where the data itself is a flag in ASCII. The player must detect and analyze this data stream.
    * **Phase 1: The Light Show Conundrum (In-Game BENDER - Exploration, Item Use, NPC Interaction)**
        *   **Locations:**
            *   `VegasSphere` (new NPC: "Pixel Pete")
            *   `ParkingLot` (Pocket Logic Analyzer, hidden in a "Glove Compartment" of a parked car)
        *   **Description:**
            1.  Player explores the `VegasSphere`, where a dazzling, ever-shifting LED display dominates the environment. Here, they encounter a new NPC, **Pixel Pete**, a quirky lighting tech with a backpack full of RGB LED strips and a penchant for cryptic advice.
                *   *Pixel Pete Description:* "A gangly figure with LED glasses and a vest that pulses to an inaudible beat. Pete seems to be in constant motion, fiddling with a tangle of wires and muttering about 'data streams' and 'hidden rainbows.'"
            2.  Talking to Pixel Pete, the player receives a hint:  
                *   "You ever notice how the last light in a chain sometimes glows a little different? Some say it’s just a glitch, but I think it’s got secrets to tell-if you know how to listen. The trick is catching the flow, not just the glow. Sometimes, the real magic happens just above the poster-where the circuit’s story ends, but the message keeps going."
            3.  Pixel Pete mentions he’s missing his favorite tool, a **Pocket Logic Analyzer**, which he lent to a friend who was hacking on badge LEDs in the parking lot and never got it back.
            4.  Player travels to the `ParkingLot` and examines a "Glove Compartment" in a suspiciously sticker-covered, parked car. Inside, they find the **Pocket Logic Analyzer**.
                *   *Pocket Logic Analyzer Description:* "A compact device with a tiny screen and a rainbow ribbon cable. Its display cycles through cryptic waveforms and binary numbers. A sticker on the back reads: 'For those who see the music in the light.' Scribbled on a piece of tape stuck to the side is the string: 'V1MyODEyQg=='."
            5.  Player takes the Pocket Logic Analyzer and returns to Pixel Pete at the VegasSphere.
            6.  Giving the Pocket Logic Analyzer to Pixel Pete, he thanks the player and offers a more direct hint:
                *   "With this, you can finally see what the lights are *saying*, not just what they’re *showing*. Sometimes, the last pin in the chain is more than an endpoint-it’s a mouthpiece. If you tap into it, you might just hear a song in the static. The protocol’s not just any old light show-think Neo, think Pixel, and you’ll be on the right wavelength."
            7.  If the player examines the Pocket Logic Analyzer before giving it to Pete, they see a cryptic readout:  
                *   "The analyzer flickers, displaying a repeating pattern: short bursts, long pauses, and a string of numbers that seem oddly familiar-almost like ASCII codes, but with a twist."
        *   **Key Objects Required:** Pocket Logic Analyzer (from ParkingLot, hidden in Glove Compartment), interaction with Pixel Pete (VegasSphere).
        *   **Output/Hint:** The player learns that the last pin in the LED chain (subtly referenced as being "just above the poster") outputs a data stream that can be captured and decoded, and that the protocol is related to NeoPixel. The base64 string 'V1MyODEyQg==' (which decodes to "WS2812B") is found on the analyzer itself. The hints nudge the player toward capturing and analyzing the data stream from the last LED pin, and to recognize the protocol and extract the message.
    * **Phase 2: The Rainbow Protocol (IRL Hardware Hacking, Protocol Analysis)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1.  Following the in-game hints, the player identifies the hidden pin above the poster as the output from the last LED in the WS2812B (NeoPixel) chain.
            2.  Player connects a logic analyzer (or similar device) to this pin.
            1.  Following the in-game hints, the player identifies the hidden pin above the poster as the output from the last LED in the WS2812B (NeoPixel) chain.
            2.  Player connects a logic analyzer (or similar device) to this pin.
            3.  The badge is automatically sending extra LEDs worth of data to this pin in the background.
            4.  Player captures the data stream from the pin using the logic analyzer.
            5.  Player analyzes the captured data, recognizing the protocol as WS2812B/NeoPixel.
            6.  Player writes or uses a script/tool (e.g., Python, Saleae Logic, or tools/sk6812_solder.py) to decode the timing of the captured data into binary, then ASCII.
            7.  The decoded ASCII string is the flag.
        *   **Key Objects Required:** Logic analyzer (or similar), script/tool for WS2812 decoding, knowledge of ASCII.
        *   **Output/Hint:** The flag, revealed by decoding the extra data sent to the last LED pin.
    * **Flag:** `flag{r9b_y0u_f0und_me!}` (padded to align with 24-bit colors...yes we can tunnel hidden messages over LEDs)
    * **CTF Points (Hard):** 85
    * **CTFd_Challenge_Name:** `L3D_P00p_5h007`

 * Phase 2 - 07 Thermistor Heat (In-Game Interaction)
    * **Overall:** Players must find a hamster object in one of the available areas and place it in a microwave located in the Ramen Noodle House. This action will provide hints about a thermistor and activate a hidden locust UART.
    * **Phase 1: Hamster and Microwave Puzzle (In-Game BENDER - Exploration, Item Use)**
        *   **Location:** Sketchier Alleyway (Hamster Object) and Ramen Noodle House (Microwave Object).
        *   **Description:**
            1.  Player explores the Sketchier Alleyway and finds the **Hamster Object**. Its description hints at its potential use in a puzzle.
                *   *Hamster Description:* "A small, fluffy hamster. It looks oddly calm. You can't help but think it probably tastes like Karaage...If found, return to Weird Ed at 14 Ramenwood Drive"
                *   *This is also a nod and hint to Maniac Mansion, written in SCUMM, that was influenced by the original Zork games and Inform.*
            2.  Player takes the **Hamster Object** to the **Microwave Object** located in the Ramen Noodle House.
                *   *Microwave Description:* "A slightly old microwave. It hums faintly when powered on."
            3.  Player uses the command `PUT HAMSTER IN MICROWAVE`.... then `TURN ON MICROWAVE` to interact with the microwave...if they forget to CLOSE the microwave door...well lets leave that easter egg for another time.
            4.  Upon successful interaction, the microwave activates and provides the clue about the thermistor and locust UART:
                *   *Clue:* "Thermistors are like resistors with no markings. Heat reveals their secrets."
                *   *Hint:* Locusts swarm the area, buzzing in a peculiar pattern that hints at the hidden locust UART.
        *   **Key Objects Required:** Hamster Object (found in Sketchier Alleyway).
        *   **Output/Hint:** The clue about the thermistor and the hidden locust UART.
    * **Phase 2: Locust UART Activation (Hardware Hacking, Serial Communication)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1.  Locusts swarm the Ramen Noodle House after the microwave interaction, buzzing in a pattern that hints at the UART interface.
            2.  Player inspects the badge PCB for unpopulated headers or test points that might indicate a UART interface (transmit, receive, ground, power).
            3.  Player uses a terminal emulator (e.g., minicom, PuTTY) to connect to the serial port, determining the correct communication rate.
            4.  Upon successful connection, the UART interface displays a loop with the name of the challenge and flag. If the temp normalizes the flag disappears.
        *   **Key Objects Required:** Soldering iron/probes, USB-to-Serial adapter, terminal software.
        *   **Output/Hint:** A flag or additional hints for other hardware challenges.
    * **Flag:** `flag{h0t_en0ugh_2_s0lder}`
    * **CTF Points (Medium):** 65
    * **CTFd_Challenge_Name:** `M4n14c_N00dl3_M4n510n`

* Phase 2 - 08 Badge Hardware Hacking - Thermistor (Temperature Sensor) COLD
    * **Overall:** Player needs to manipulate a temperature sensor on the badge by chilling it to trigger an event or reveal a flag.
    * **Phase 1: Cooling Down the Heat (In-Game BENDER - Exploration, Item Use, NPC Interaction)**
        *   **Locations:**
            *   `VegasStripNorth` (new scenery: "Overheated PC").
            *   `StreetEast` (new scenery: "Discarded Electronics Pile" containing "Thermal Paste").
            *   `VegasStripSouth` (new scenery: "Abandoned Tech Booth" containing "CPU Cooler").
        *   **Description:**
            1.  Player explores the `VegasStripNorth` and examines an "Overheated PC" baking in the hot Vegas sun. The PC won't turn on, and its description hints at needing repairs:
                *   *Overheated PC Description:* "A dusty desktop PC sits on the sidewalk, its case hot to the touch under the relentless Vegas sun. The monitor displays nothing but a faint flicker, and the CPU fan is missing entirely."
            2.  Player explores the `StreetEast` and examines a "Discarded Electronics Pile" near a dumpster. Searching it reveals "Thermal Paste".
                *   *Discarded Electronics Pile Description:* "A heap of broken electronics lies abandoned near a dumpster. Among the shattered screens and tangled wires, a small tube of thermal paste catches your eye."
                *   *Thermal Paste Description:* "A small tube of thermal paste, slightly dented but still usable. The label reads: Cooler Master - Stay Frosty!"
            3.  Player explores the `VegasStripSouth` and examines an "Abandoned Tech Booth" near a closed convention center. Searching it reveals a "CPU Cooler".
                *   *Abandoned Tech Booth Description:* "A dilapidated booth with faded banners advertising high-performance PC components. Among the scattered debris, a CPU cooler lies forgotten."
                *   *CPU Cooler Description:* "A compact CPU cooler with slightly bent fins. A sticker on the side reads: Arctic Chill - Keep Your Cool!"
            4.  Player must take both the "Thermal Paste" and the "CPU Cooler" and return to the "Overheated PC" in the `VegasStripNorth`.
            5.  Player uses the "Thermal Paste" and "CPU Cooler" on the "Overheated PC" (e.g., `USE THERMAL PASTE ON PC` followed by `USE CPU COOLER ON PC`).
                *   *Interaction Output:* "You carefully apply the thermal paste to the CPU and install the cooler. The PC hums to life, and the monitor flickers on. As the screen stabilizes, you notice a small, metallic locust crawling across the monitor's edge, its antenna twitching as if transmitting something. A message appears: Thermistors are like resistors with no markings. Cold reveals their secrets."
            6.  After completing this interaction, the player receives the hint about the thermistor and the need to chill it to reveal the flag.
        *   **Key Objects Required:** Thermal Paste (from StreetEast), CPU Cooler (from VegasStripSouth).
        *   **Output/Hint:** The clue about the thermistor: "Thermistors are like resistors with no markings. Cold reveals their secrets." The locust crawling on the monitor hints that Phase 2 - 01 (Hidden UART) must be completed to access the flag.
    * **Phase 2: Sensor Reconfiguration & Environmental Trigger (Hardware Hacking, Environmental Interaction)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1.  Following the in-game hint, player identifies the thermistor on the badge PCB.
            2.  Player uses a method to significantly lower the temperature of the thermistor (e.g., using canned air upside down, ice pack).
            3.  Chilling the thermistor triggers a specific behavior on the badge (sending a signal w/ flag via UART).
        *   **Key Objects Required:** Knowledge of Phase 2 - 01 Hidden UART, method to chill the thermistor (e.g., canned air, ice pack).
        *   **Output/Hint:** A flag revealed through Hidden UART after chilling the temperature sensor. If the temp normalizes the flag disappears.
    * **Flag:** `flag{c0ld_j01nt3d}`
    * **CTF Points (Medium):** 69
    * **CTFd_Challenge_Name:** `Ch1ll_Thr1lls_and_CPU_Sp1lls`

* Phase 2 - 09 Badge Hardware Hacking - Hall Effect (Magnet Sensor)
    * **Overall:** Player needs to interact with the badge's accelerometer by tapping a specific rhythm or sequence to unlock a feature or reveal a flag. The in-game portion guides the player to the physical badge and provides all the context needed to solve the hardware challenge.
    * **Phase 1: The Magnetic Mystery (In-Game BENDER - Exploration, Item Use, NPC Interaction)**
        *   **Locations:**
            *   `LVCCFoodCourt` (new scenery: "Janitor's Mop Bucket" containing "Gold Matt Damon Keychain")
            *   `DC33ContestArea` (new scenery: "Prize Capsule Machine" containing "Horseshoe Magnet")
            *   `LVCCMainHall` (new NPC: "Professor Flux")
        *   **Description:**
            1.  Player explores the `LVCCFoodCourt` and examines a "Janitor's Mop Bucket" left near a table. Searching it reveals a "Gold Matt Damon Keychain" submerged in the murky water.
                *   *Gold Matt Damon Keychain Description:* "A surprisingly weighty keychain in the shape of Matt Damon, plated in gold paint. A few small, metallic locusts crawl aimlessly across its surface. It feels faintly magnetic and has a tiny inscription: 'For the true Hall of Fame.'"
            2.  Player explores the `DC33ContestArea` and examines a "Prize Capsule Machine" in the corner. Searching it reveals a "Horseshoe Magnet".
                *   *Horseshoe Magnet Description:* "A classic red-and-silver horseshoe magnet, the kind you'd expect to see in a cartoon. It's surprisingly strong for a prize, and a sticker on it reads: 'WARNING: Not for use near badges or credit cards.' A few small, metallic locusts crawl aimlessly across its surface."
            3.  Player must take both the "Gold Matt Damon Keychain" and the "Horseshoe Magnet".
            4.  Player brings both items to the `LVCCMainHall`, where they encounter a new NPC: **Professor Flux**.
                *   *Professor Flux Description:* "A wild-haired, lab-coat-wearing scientist with a badge lanyard covered in rare pins. He’s surrounded by a pile of technical manuals and is intently watching the crowd for anyone carrying unusual hardware."
            5.  Talking to Professor Flux before having both items results in a generic greeting and a comment about "the magnetic personalities at DEF CON."
            6.  Once the player has both the Gold Matt Damon Keychain and the Horseshoe Magnet, talking to Professor Flux triggers a special interaction:
                *   "Ah! I see you've found the legendary Matt Damon keychain and the elusive Horseshoe magnet. Did you know that some badges hide their secrets behind invisible forces? Sometimes, the key is to bring the right magnet close to the right face. But beware-only the golden one will reveal the truth. The Hall effect is subtle, but unmistakable for those who know where to look. Of course, you won't see anything unless the locusts are already whispering their secrets. Try it near Matt Damon himself, and watch for a sign."
            7.  If the player examines the Gold Matt Damon Keychain after this, they notice a new detail: "A tiny arrow on the back points toward Matt Damon's face, and the gold paint seems slightly worn there."
        *   **Key Objects Required:** Gold Matt Damon Keychain (from LVCCFoodCourt, hidden in Janitor's Mop Bucket), Horseshoe Magnet (from DC33ContestArea, hidden in Prize Capsule Machine), interaction with Professor Flux (LVCCMainHall).
        *   **Key Objects Required:** Gold Matt Damon Keychain (from LVCCFoodCourt, hidden in Janitor's Mop Bucket), Horseshoe Magnet (from RamenNoodleHouse, hidden in Prize Capsule Machine), interaction with Professor Flux (LVCCMainHall).
        *   **Output/Hint:** The player learns that the badge has a Hall effect sensor hidden behind the Matt Damon face, that using a magnet near that spot will trigger a secret, and that the Hidden UART challenge is a prerequisite.
    * **Phase 2: The Magnetic Reveal (IRL Hardware Hacking, Hall Effect Sensor)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1.  Following the in-game hints, the player identifies the Matt Damon face on the badge as the likely location of the Hall effect sensor.
            2.  Player brings a magnet close to the Matt Damon face on the badge.
            3.  The badge detects the magnetic field via the Hall effect sensor.
            4.  If the correct spot is triggered, and the UART challenge (Phase 2 - 01) is active, the badge reveals the flag (e.g., via UART, on the screen, or through LEDs).
        *   **Key Objects Required:** Magnet, knowledge of the Hall effect sensor location.
        *   **Output/Hint:** The flag, revealed by triggering the Hall effect sensor with a magnet at the correct location.
    * **Flag:** `flag{th3_magnet_1d3ntity}`
    * **CTF Points (Medium):** 65
    * **CTFd_Challenge_Name:** `H4ll_M0n1t0r`

  * Phase 2 - 10 Maine Isn't Real
    * **Overall:** Player interacts with Zapp at Circus Circus, who discusses Maine as a conspiracy. Zapp sends the player west into a maze.
    * **Phase 1: Conversing with Zapp (In-Game BENDER - NPC Interaction)**
        *   **Location:** Circus Circus (west of LVCC Loop North Entrance).
        *   **Description:**
            1.  Player goes west from LVCC Loop North Entrance to Circus Circus.
            2.  Player interacts with Zapp, who is hanging out at the Burger King counter.
            3.  Zapp discusses Maine, calling it a conspiracy and hinting at something deeper.
            4.  Zapp sends the player west into a maze, saying, "If you want to uncover the truth about Maine, you'll need to navigate the labyrinth. But beware-what you find might change everything you thought you knew."
        *   **Key Objects Required:** None, relies on interaction with Zapp.
        *   **Output/Hint:** Zapp provides cryptic hints about Maine and directs the player west into the maze.
    * **Phase 2: The Maze (Exploration)**
        *   **Location:** West of Circus Circus.
        *   **Description:** Its a maze and its really really really confusing and hard to get through. We even made it so going forward and backward results in different placement. Overall the challenge is getting through the horrible maze. Also this thwarts attempts to use AI to play the game as they get stuck in our mazes...
        *   **Key Objects Required:** Slackware Disks
        *   **Output/Hint:** Install Disks in 486 Computer. Enter HHHHHHHH in to 5n4k3y for the flag.
    * **Flag:** `flag{Matt_greater_than_systemd}`
    * **CTF Points (Medium):** 52
    * **CTFd_Challenge_Name:** `M41n3_1sn7_R34l`
  
  * Phase 2 - 11 Badge Hardware Hacking - Backspace Character Secret Flag Printed from Hidden UART
    * **Overall:** A flag is printed when the player connects over serial to the hidden UART, but the flag is immediately followed by many backspace characters, which obfuscate the flag and prevent it from being visible in the terminal.
    * **Phase 1: Discovering the Clues (In-Game Exploration and Observation)**
        *   **Location:** DC33 Talk Tracks, LVCC West Entrance, LVCC South Entrance, DC33 Merch Line.
        *   **Description:**
            1. The player encounters an NPC named **Keymaster** in the DC33 Talk Tracks area. Keymaster is preparing to give a DEF CON talk but cannot demo their exploit because their keyboard is broken. Keymaster asks the player to find a working keyboard.
            2. The player searches the **Pile of Broken Keyboards** scenery in the LVCC West Entrance area. Upon searching the pile, the player finds a single working keyboard, which is missing the backspace key. The player takes the keyboard and brings it back to Keymaster.
            3. Keymaster refuses the keyboard, complaining about the missing backspace key. Keymaster mentions seeing someone selling custom keycaps in front of the LVCC South Entrance.
            4. The player goes to the LVCC South Entrance area and finds an NPC named **Shade**, a mysterious figure wearing a trenchcoat and selling various items. When asked about the backspace keycap, Shade offers a custom locust-themed backspace keycap but demands a collectible DC33 pin from the merch store in exchange.
            5. The player navigates through the lengthy merch line in the DC33 Merch Line area and eventually obtains the collectible DC33 pin.
            6. The player returns to Shade in the LVCC South Entrance area and trades the DC33 pin for the custom locust-themed backspace keycap.
            7. The player uses the locust-themed backspace keycap on the keyboard in their inventory to create a completed keyboard.
            8. The player brings the completed keyboard back to Keymaster in the DC33 Talk Tracks area. Keymaster accepts the keyboard and successfully prepares for their talk.
            9. As a reward, Keymaster provides the player with a hint about how to solve the hardware hacking challenge. The hint involves locust-themed imagery and references specific hardware manipulation techniques.
    * **Phase 2: Physical Interaction with the Badge (IRL Hardware Hacking)**
        *   **Location:** IRL - The Badge Hardware.
        *   **Description:**
            1. Following the in-game hints, the player knows there is a secret message involving the hidden UART on the badge (hinted at by in-game locust) and backspaces.
            2. The player finds a way to see the raw bytes being sent from the serial port on the hidden UART, logic analyzer, or serial monitor with no translation just raw bytes.
            3. The player translates the raw bytes back to ASCII and sees the full message.
            4. Full message printed from the hidden UART over serial: 'flag{n3gaT1ve_SPAC3}\b\b\b\b\b\b\b\b\b\b\b\b\b\b\b\b\b\b\b\b'
            5. This was inspired by the amazing work at LayerOne for the Intercept hardware hacking challenge.
    * **Key Objects Required:**
        *   Working Keyboard (assembled with locust-themed backspace keycap).
        *   Collectible DC33 Pin.
    * **Output/Hint:** The flag, discovered by receiving raw bytes from the serial port.
    * **Flag:** `flag{n3gaT1ve_SPAC3}`
    * **CTF Points (Medium):** 57
    * **CTFd_Challenge_Name:** `k3Yb04rD_W4RR10r`

**Phase 2 - 12 False Flag Operations**  
* **Description:** These are devious traps disguised as challenges. Each flag awards negative points in CTFd, with a total penalty of -750 points for solving all five. However, for those daring enough to take the risk, a sixth challenge unlocks upon completing the first five. This final challenge offers a positive flag worth 1000 points, netting a 250-point gain if successful-or leaving players in a deeper hole if they fail. The player does not know how many there are, so they have to complete it all or they are SOL. 
* **Flags:**  
  * **Flag 0 - 4LPH4:** Found in `bender0.z5` as compiled z machine code, which must be decompiled and inspected, requiring a flash chip dump and inspection: `flag{D0_not_US3_shAr3war3_fl4g}`  
  * **Flag 1 - BR4V0:** Found in `bender1.z5` as compiled z machine code, which must be decompiled and inspected, also requiring a flash chip dump and inspection: `flag{D0_not_THIS_fl4g_e1th3R_i7_dumB}`  
  * **Flag 2 - CH4RL13:** Found in `bender2.z5` as compiled z machine code, which must be decompiled and inspected, requiring similar methods: `flag{Seriously_D0_not_N07_n0t_US3_fl4g}`  
  * **Flag 3 - D3LT4:** Hidden in the compiled firmware, discoverable through firmware dumping: `flag{D0_not_US3_th1s_fl4g}`  
  * **Flag 4 - 3CH0:** Hidden in the build path of the firmware which is compiled in, requiring a memory dump via a debugger and using OpenOCD: `flag{/leeroy_jenkins/DoNotUse_this_negative_flag/}`  
  * **Flag 5 - F0XTROT:** Given to them as a reward in CTFd: `flag{WH15KY_T@NG0_TH4T_W4S_R1SKY_AF_C0NGR4TS}`
  * **CTFd_Challenge_Name:** `F4L53_FL4G_0P3R4710N`  
  * **CTF Points:** -150 for each of the first five flags (-750 total), then +1000 for the final flag, to net 250 points if all are solved.  

**Phase 2 - 13 Hug 5n4ck3y**  
* **Description:** Just a small fun challenge to hug 5n4ck3y. You finally make it to the snack machine at the end, interact, you notice the memorable screen of No Physical Attacks, No Assholes, Have Fun, Hugs Welcome. If you attack 5n4ck3y, you die game over. If you hug 5n4ck3y, you get a flag. Many people ran up and hugged 5n4ck3y which made us happy.
  * **CTFd_Challenge_Name:** `F1N4L_3XPL01T_0F_K1NDN3SS`
  * **Flag:** `flag{k!ndn355win5}`
  * **CTF Points:** 21