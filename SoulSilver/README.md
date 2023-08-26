# ROM Hacking guide for Pokémon SoulSilver

## Objectives

My objective is to tweak a few things in pokémon SoulSilver to improve its quality of life. I want to achieve the following changes:
- Be able to use HM moves in the overworld (as opposed to combat!) without a pokemon having learned the move. So I don't need to carry around a HM mule or teach useless moves to my fellow pokémons
- Be able to reuse TMs

## Prerequisites

- Have a ROM of the Pokémon SoulSilver game (NDS format)
- [DS Pokemon Rom Editor Reloaded](https://github.com/AdAstra-LD/DS-Pokemon-Rom-Editor) (DSPRE)
- [Preferrable] Have an emulator and a save file to be able to test your changes

## Notice

I'll use a french version of the SoulSilver game and I don't know how different it would be in other languages, but I expect the changes we'll perform to be almost identical.

## DSPRE

Open the ROM with DSPRE and go to the Script Editor tab. This is where we're gonna fiddle around to improve our game.  
Use the Search feature to query a few words that could be interesting. After some time looking on how to edit the Cut move in the overworld, I come accross the `CutAnimation` command. This looks promissing.

It looks like most of the usages of `CutCommand` are located in `File 146`. After some digging around, we also notice a few other interesting commands: `CheckMoveInParty` and `CheckBadge`.

Let's have a look at the `Script 1`:
```cs
Script 1:
	PlayFanfare 1500
	LockAll 
	FacePlayer 
	CheckMoveInParty 0x800C 15
	CompareVarValue 0x800C 6
	JumpIf EQUAL Function#1
	CheckBadge 1 0x800C
	CompareVarValue 0x800C 0
	JumpIf EQUAL Function#1
	Message 0
	OpenTouchScreen 
	YesNoTouchScreen 0x800C
	CloseTouchScreen 
	CompareVarValue 0x800C 0
	JumpIf EQUAL Function#2
	CloseMessage 
Jump Function#3
```

`CheckMoveInParty 0x800C 15` would check that the move `15` is available in our party, and store that at the address `0x800C`, right? Let's confirm that with an external documentation... :eyes: and yes, indeed! `15` corresponds to the Cut move!
`CheckBadge 1 0x800C` and would be about checking whether we have successfully obtained the second badge, isn't it?





TODO: Rock smash


```cs
Script 2:
	PlayFanfare 1500
	LockAll 
	FacePlayer 
	CheckMoveInParty 0x800C 249
	SetVarFromVariable 0x8004 0x800C
	CompareVarValue 0x800C 6
	JumpIf EQUAL Function#4
	CheckBadge 0 0x800C
	CompareVarValue 0x800C 0
	JumpIf EQUAL Function#4
	Message 3
	OpenTouchScreen 
	YesNoTouchScreen 0x800C
	CloseTouchScreen 
	CompareVarValue 0x800C 0
	JumpIf EQUAL Function#5
	CloseMessage 
Jump Function#3
```