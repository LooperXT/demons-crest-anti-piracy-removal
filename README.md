# Demon's Crest Anti-Piracy Removal

This project contains .bps and .ips patches that remove the anti-piracy checks and related anti-features from Demon's Crest.

## Supported ROMs

Demon's Crest (Europe).sfc  
MD5: d8470882550b0d229039a02f691949fb

Demon's Blazon - Makaimura Monshou Hen (Japan).sfc  
MD5: 98a7294710502951f4a4bc92a703acd8

Demon's Crest (USA).sfc  
MD5: e990ae600a8326eda25489bdddffca55

## The Four Anti-Piracy Checks

Anti-Piracy checks take place at four times throughout Demon's Crest: when the Capcom logo is displayed shortly after powering on the system, when headbutting the Hippogriff statue in Stage 1, when approaching Arma in Stage 1, and whenever Firebrand takes damage. The game checks for abnormal ROM mapping and for the presence of SRAM. If a check is failed, a value of $FF is written to a corresponding address in the range of $7E:0EEB-$0EEE.

## The Anti-Features

If a non-zero value is found at $7E:0EEB or $7E:0EED, the player will not be able to open the inventory menu. After the player presses Start, a BNE instruction will skip over the logic that results in the inventory menu being opened, straight to the RTL at the end of the relevant subroutine.

If a non-zero value is found at $7E:0EEC or $7E:0EEE, enemies will not take damage. Firebrand's projectiles will land and may cause an enemy to be knocked back or stunned, but a BNE instruction will skip over the HP subtraction steps.

## Changes

This patch changes precisely eight bytes in the game code to circumvent the anti-piracy checks and to neutralize the anti-features. It does not make any other changes.

Following the four anti-piracy checks, there are conditional BNE and BEQ instructions that lead down the good path if a check was passed. These have been turned into unconditional BRA instructions so that the check is always passed and the good path taken, resulting in the values at $7E:0EEB-$0EEE never being overwritten from their defaults of $00.

In addition, the offsets for BNE redirects in the "open menu" and "reduce enemy HP" subroutines have been changed, so that the BNE lands on the good path, and not the bad path. This means that even if non-zero values have found their way to the above addresses somehow (e.g., when loading an old save state), the player will still be able to open the menu and to damage enemies.

| EU Address | JP Address | NA Address | Original Byte | Patched Byte |
| :--------- | :--------- | :--------- | :------------ | :----------- |
| $80:8767   | $80:8753   | $80:8767   | $D0           | $80          |
| $80:E56E   | $80:E550   | $80:E56E   | $D0           | $80          |
| $82:9B3B   | $82:9B3B   | $82:9B3B   | $F0           | $80          |
| $BE:E35E   | $BE:E3CB   | $BE:E35E   | $F0           | $80          |
| $84:8A3A   | $84:8A3A   | $84:8A3A   | $32           | $06          |
| $84:8A40   | $84:8A40   | $84:8A40   | $2C           | $00          |
| $82:88FA   | $82:88FA   | $82:88FA   | $17           | $06          |
| $82:8900   | $82:8900   | $82:8900   | $11           | $00          |

## Usage

Using a patching tool like FLIPS or beat, apply either a .bps patch or a .ips patch to your headerless and legally obtained ROM dump. Different patches have been provided for different regions. They consist only of binary offset deltas and contain no copyrighted data.

## Further Reading

More details can be found in my [Demon's Crest guide on GameFAQs](https://gamefaqs.gamespot.com/snes/588275-demons-crest/faqs/82809).
