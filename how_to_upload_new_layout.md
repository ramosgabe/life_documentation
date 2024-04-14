# How to Upload New Layout


Items Needed:
1. Corne Keyboard
2. VIA
3. VS Code or some other compiler
4. Little knowledge of how to use terminal
5. homebrew installed (if needed)

References:
1. https://docs.qmk.fm/#/newbs_building_firmware
2. https://docs.qmk.fm/#/newbs_flashing
3. https://raw.githubusercontent.com/manna-harbour/miryoku/master/data/configurator/miryoku-configurator-crkbd.json
4. https://brew.sh/
5. https://docs.qmk.fm/#/feature_advanced_keycodes?id=modifier-keys

Okay so I decided to make this documentation to guide the people who are not necessarily full-time programmers, but for the everyday person who is computer literate and enjoys a little bit of code on the side. 

My target audience, who are people like me, who purchased a pre-built ergonomic split keyboard, for the intentions of well, ergonomics. I'm hoping to save someone the time that I spent finding and installing the "best" keyboard layout, and combing through reddit and github until 2:30am to then feel defeated that what I found wasn't working. 

I decided to go with one of the most used layouts, the miryoku layout. This has been a huge game changer for me with this keyboard, with everything seeming to be in the perfect position. If this layout doesn't work for you, I'll show you how to modify the layout to your own needs.

This is also to prevent the user from bouncing back and forth from different references. I'll link those references directly, but for the most part, we'll be following QMK documentation. 

Finally, I know that the "best" layout and the program that they use is relative to each person. This process just seemed the most straight-forward to me. With all that being said, please take your time with this procedure, and ask questions if a step is unclear. 

Feel free to modify your layout, as you see fit.

Enjoy!

## Easiest Method

- [ ] Install [homebrew](#install-homebrew)
- [ ] Install [QMK CLI](#install-qmk-cli)
- [ ] Download the `config.h`, `keymap.c`, and `rules.mk` files
- [ ] Create a new folder in the `/qmk_firmware/keyboards/crkbd/keymaps`
	- [ ] I'd recommend making a second copy of the VIA folder, just in case you want to go back to the default state
- [ ] Place the previously downloaded files into the newly created keymap folder
- [ ] Assign one key as the `Reset` key on your keyboard
- [ ] Open up terminal and input the following command:
	- [ ] `qmk flash -kb crkbd -km <whatever you named your keymap file>`
- [ ] Terminal will then compile your firmware and display something like `Waiting for USB serial port - reset your controller now`
- [ ] Press and hold the reset button until you see terminal starting to flash your keyboard
- [ ] Unplug your keyboard and plug into other half to flash the other controller(following the same steps to flash)
- [ ] Once both sides are properly flashed, open up VIA to confirm that it worked
	- [ ] Note: There seems to be a bug with VIA that if some layers do not have assignments, the webpage will be stuck on the loading screen. If this happens, just refresh the web page and VIA should then load
- [ ] Enjoy!



# Longer Method:
## Prep Build Environment

### Install Homebrew


- [ ] Open up a new terminal window
- [ ] Run the following command:
```
/bash/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Install QMK CLI


- [ ] Either in the same terminal, or a new one, run the following command:
```
qmk setup
```

>[!Note] 
Per the QMK documentation, "In most situations you will want to answer `y` to all of the prompts."
> 
>The qmk home folder can be specified at setup with `qmk setup -H <path>`, and modified afterwards using the [cli configuration](https://docs.qmk.fm/#/cli_configuration?id=single-key-example) and the variable `user.qmk_home`. For all available options run `qmk setup --help`.

## README

>[!WARNING] 
>PLEASE NOTE THAT THE FOLLOWING STEPS DO NOT NEED TO BE COMPLETED IN ORDER TO CREATE YOUR FIRMWARE. PLEASE START AT [EITHER CREATING THE FIRMWARE STEP](#building-the-firmware) OR IF YOU FEEL MORE COMFORTABLE [CREATING A NEW KEYMAP](#prep-build-environment) IN THE CRKBD FOLDER KEYMAP FOLDER YOU MAY DO SO. JUST COPY AND PASTE THE FOLLOWING FILES INTO YOUR NEW KEYMAP FOLDER AND FLASH YOUR KEYBOARD HERE.

## Prepping Build Environment (Cont.)
### Test Build Environment
- Okay, so the next few steps will be creating a keymap for your specific keyboard. I'm using a Corne 42 layout keyboard. Yours may be different so try to find the folder to verify that it was indeed downloaded to your computer

- For my Corne, I compiled the firmware by using the `crkbd` folder with the following command:
```
qmk compile -kb crkbd -km <insert_new_folder_name_here)
```


>[!Note] 
>If you have a different keyboard the syntax looks like this:
> `qmk compile -kb <keyboard> -km default`
> 
> Just insert the name of the folder that is found under the `qmk_firmware/keyboards/`

 - Per the QMK docs, the output should look similar to this:
```
```
`Linking: .build/clueboard_66_rev3_default.elf [OK] Creating load file for flashing: .build/clueboard_66_rev3_default.hex [OK] Copying clueboard_66_rev3_default.hex to qmk_firmware folder [OK] Checking file size of clueboard_66_rev3_default.hex [OK] * The firmware size is fine - 26356/28672 (2316 bytes free)`
```
```

## Building the Firmware

- Now we are going to be building the firmware that will be flashed to your keyboard. To do this, we must create a new folder in our keyboard's keymap folder.

### Creating a New Keymap

- First we will set the keyboard as our default:

- [ ] Run the following command in terminal:
```
qmk config user.keyboard=crkbd
```

- [ ] Then set your default keymap name. Name it whatever you'd like, but make it something that you'll remember, or write it down. 
```
qmk config user.keymap=<insert_keymap_name_here>
```
- [ ] Next, run the following command to create the keymap that will populate in the keymaps directory(folder):
```
qmk new-keymap
```

>[!Note] 
>If you didn't set a default keymap name, that's fine! Create a new one by running: 
>
>`qmk new-keymap -kb <keyboard_name> -km <keymap_name> `

## Adding Layers to VIA
- So this is where things get fun! We get to add more layers to VIA, and luckily, this is super easy. I think the default firmware only gives the user 4 layers to work with, but if you're using the miryoku layout, or any other layout that requires more, this next part will give you more layers if your keyboard has the memory for it. 
- The easiest way to do this is to either copy and paste this code into a text editor like VS Code or download the file off of git.

- [ ] Download/Copy files
- [ ] Move/Save the files in the folder named via
	- [ ] You may get asked if you want to replace the existing files, select replace. 

>[!Note] 
>If you need more than 7 layers, all you need to do is open up the `keyboard.c` file in VS Code and copy and paste another empty layout. 
>
>That being said, your keyboard can only hold so many layers, so don't go overboard. 
>
>I stuck with the 7 layers and essentially have an empty layer to experiment with since I didn't need the layer with all of the function keys.


## Flashing Firmware

>[!Warning]
>Before you flash the firmware, ensure that you assign one of the keys on VIA to Reset. You'll need this to place your keyboard in bootloader mode

- [ ] Assign one key to `Reset`
- [ ] Type/Copy&Paste the following command into terminal
	- [ ] Either type:
```
qmk flash
```


- [ ] Or use the following if you're not sure if you compiled your firmware correctly

```
qmk flash -kb crkbd -km <whatever you named your keymap file>
```
- Note: if you are using a different style keyboard replace `crkbd` with the keyboard you're using


## Putting It All Together

- Now all we need to is open up VIA on our browser and all of the layers you could ever dream of, will be there.
- If you are interested in using the miryoku layout, which utilizes the Colemak-DH style layout, I'll leave that in the keymap folder on github.
	- Download the file and upload it to your keyboard through VIA



## Extras

- Now that the procedure is over, take a deep breath, pat yourself on the back. You are now a programmer! (joking)

- If you feel like you don't enjoy the layout provided, feel free to modify whatever you want!
- If you want to re-map keys that seem to not exist in the VIA GUI, save your current layout to your computer and open it with VS Code.
	- You can now edit whatever key you want and use this documentation *provide link* to edit any keys
>[!Tip]
>The way VIA maps the keys are weird. Try to find the pattern. For mine it was mapped like this:
>
>Left Split:
> 1, 2, 3, 4, 5, 6
> 7, 8, 9, 10, 11, 12
> 13, 14, 15, 16, 17, 18
> KC_NO, KC_NO, KC_NO, 19, 20, 21 
> 
>Right Split:
>6, 5, 4, 3, 2, 1
>12, 11, 10, 9, 8, 7
>18, 17, 16, 15, 14, 13
>KC_NO, KC_NO, KC_NO, 19, 20, 21
>
>As long as you input the keys in that order in the .json file then everything should be good. If there is an error, make sure that you have the proper number of keys. KC_NO means there is no physical key in that spot.

- Another quick note, the modifier keys to use the mod-tap function has different syntax for VIA than QMK. To use these mod-tap function keys the syntax `MT(<keystroke_1> | <keystroke_2>)` is to be used

If there are any issues with this guide, please let me know, because I am just a human. 

Much Love


