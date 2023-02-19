# Mechanics
1. [Running](#running)
2. [Sliding](#sliding)
3. [Swimming](#swimming)
4. [Stamina](#stamina)
5. [Energy](#energy)
   
   5.1 [Energy depletion](#energy-depletion)
   
   5.2 [Supercharged](#supercharged)
6. [Suit Modes](#suit-modes)

   6.1 [Cloak Mode](#cloak-mode)
   
   6.2 [Armour Mode](#armour-mode)
7. [Jumping](#jumping)
   
   7.1 [Power/regular jumping](#jump-types)
   
   7.2 [Slope jumps](#slope-jumps)
   
   7.3 [Flick jumps](#flick-jumps)
   
   7.4 [Coyote time/Ground Smoothing](#coyote-time)
   
   7.5 [Jump cancels](#jump-cancels)
8. [Vaulting](#vaulting)
   
   8.1 [Vault Clipping](#vault-clipping)
   
   8.2 [Vault to jump](#vault-to-jump)
9. [Grabbable Items](#grabbable-items)

   9.1 [Item clipping](#item-clipping)
   
   9.2 [Item boosting](#item-boosting)
10. [Fall damage](#fall-damage)

   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;10.1 [Heavy fall clipping](#heavy-fall-clipping)

## 1 Running <a name="running"></a>
Default running speed is 9m/s. On sloped terrain, speed decreases. It is often faster to [slide](#sliding) down slopes. For upslopes, jumping up them from preceding flat ground is faster. Jumping from the slope itself will skew the jump upwards and waste time ([Slope jump](#slope-jumps)).

Running speed is slower when:
* Strafing (moving diagonally to one side)
* Energy is fully depleted
* Damage is being taken
* Carrying a heavy weapon
* Armour mode is enabled (and sometimes when supercharged, upon dying/reloading checkpoint)

## 2 Sliding <a name="sliding"></a>
Sliding is initiated by crouching whilst running. Sliding loses speed on shallow ground, but will continuously build speed on steep enough downslopes. If sliding is initiated soon after landing from midair, velocity is maintained into the slide (allowing for chaining moves like [item boosts](#item-boosting) into slides). Sliding "sticks" the player to the ground, which can have a number of uses: Sliding and then jumping (slide jumping) can produce shallower jumps, which can make [jump cancelling](#jump-cancels) low objects more lenient. Sliding off ledges gives greater acceleration than running off, and can save time spent falling, or trigger a [hard fall](#fall-damage) more easily.  

## 3 Swimming <a name="swimming"></a>
Swimming diagonally (strafing), looking up maximally, and swimming on the surface of water, is fastest. Swim speed is fps dependent (faster at higher values), though with diminishing returns. Swimming upwards out of water enables a "jump" out of the water, maintaining velocity. Swimming can be made faster with the Endurance module (18.5m/s vs 11~m/s @60fps).

## 4 Stamina <a name="stamina"></a>
Stamina is a floating point value that drains steadily from 1 to 0 when sprinting. Once 0, sprinting is halted until stamina regenerates. Stamina quickly regenerates when not sprinting. Certain actions like jumps and slides do not drain stamina, so careful timing of such actions and management of time spent sprinting can avoid becoming exhausted. Stamina drain can be decreased with the Endurance module.

## 5 Energy <a name="energy"></a>
Energy is a floating point value of 100 and is depleted by some actions and suit functions. Energy recharges steadily when less than 100, though with a delay after any action that consumes energy (meaning spacing such actions apart can drain more energy than grouping them closely together, as the delays are minimised [confirm]). Nanosuit modules can reduce the energy required for some actions, and increase the speed of energy recharge. Actions that drain energy can be performed as long as energy is greater than 0, even if they would normally consume more energy than available [confirm].
### 5.1 Energy Depletion <a name="energy-depletion"></a>
Fully depleting energy to 0 reduces running speed, and there is an increased delay before energy regenerates again (generally undesirable). When energy is depleted, no action using energy can be performed.
### 5.2 Supercharged <a name="supercharged"></a>
Supercharged is a special "infinite armour mode"-like state, enabled after some checkpoints (interaction with ceph mindcarrier) or upon using an energy capsule. When supercharged, there is no energy drain and no damage is taken (except death triggers and the alpha ceph). Cloak cannot be enabled when supercharged.

## 6 Suit modes <a name="suit-modes"></a>
### 6.1 Cloak mode <a name="cloak-mode"></a>
Cloak mode makes prophet invisible, though still detectable to enemies when close enough. Certain actions like picking up items can momentarily disable cloak, leading to detection. During this momentary cloak deactivation state, if cloak mode is toggled off it will still be reactivated automatically after the state ends (potentially confusing). Energy drains steadily when cloaking, and faster in proportion with movement speed. Cloak energy drains faster when being shot, so full [energy depletion](#energy-depletion) is a risk.
### 6.2 Armour mode <a name="armour-mode"></a>
Armour mode absorbs all damage (except death triggers) when enabled, but drains energy and slows movement. Prophet is immune to damage until energy is fully drained or the mode is deactivated. With modules, the movement speed of armour mode can be increased at the cost of damage protection (Light Armour), or the protection can be increased at cost of speed (Heavy Armour). With the Heavy Armour module it is possible to survive a fall of any height/speed. 

## 7 Jumping <a name="jumping"></a>
### 7.1 Power/regular jumping <a name="jump-types"></a>
Holding the jump button will perform a power jump, and tapping jump will perform a regular jump. Power jumps have greater vertical velocity/height, but deplete some energy. Horizontal speed decreases when in air, so any type of jump from normal running speed is slower than simply running. Due to this implementation, the game cannot know which jump type is intended until the jump button is either released, or held sufficiently long to trigger a power jump. This can make jumping feel more delayed than in other games, as regular jumps initiate on keyup instead of keydown.
### 7.2 Slope jumps <a name="slope-jumps"></a>
The trajectory of a jump is different depending on the slope of the terrain. Jumping from an upward slope will give the jump a larger vertical component, and from a downslope, a larger horizontal component. Essentially, the jump trajectory is from the [normal](https://en.wikipedia.org/wiki/Normal_(geometry)) of the terrain jumped from. In combination with [coyote time mechanics](#coyote-time), this can give jumps unexpectedly low height when they are made near ledges, due to the downward trajectory the player takes when running off a ledge. Conversely, jumping from upslopes can give much greater height than might seem possible.
### 7.3 Flick jumps <a name="flick-jumps"></a>
When curving or flicking the camera in a direction whilst running off a ledge, more speed is built than usual. Jumping during this speed increase can give jumps with initial horizontal speed of 15m/s+. [mechanic is also possibly related to coyote time, with jumping off ledges vectoring the jump to have a larger horizontal component?]
### 7.4 Coyote time/Ground Smoothing <a name="coyote-time"></a>
Typically in games, a jump can only be made when the player is on the ground. This can make a game feel unforgiving, especially when there is complex terrain/geometry, as if the player jumps immediately after running off a ledge (even a very small one), their jump will fail. Crysis 3 (and other games) implement a mechanic to make jumping and running more lenient. Prophet glides smoothly across bumpy terrain, instead of becoming stuck on small bumps, and stays in a grounded state momentarily after running off a ledge, being still able to jump midair ([coyote time](https://www.urbandictionary.com/define.php?term=Coyote%20time)). It is often possible to maintain running atop adjacent items and terrain where it might not seem possible, due to this smoothed movement. Ground smoothing behaves differently at different framerates (see [framerate]()).
### 7.5 Jump cancels <a name="jump-cancels"></a>
There is also a mechanic [likely due to Coyote time/Ground Smoothing] which allows upward jumps to cancel when a ledge is jumped past, "snapping" Prophet to the level of the ledge. This can be useful to avoid unnecessarily overshooting a jump, and maintain full running speed for longer by taking shorter jump paths up to ledges, instead of arcing jumps over or onto the ledge.

## 8 Vaulting <a name="vaulting"></a>
Some objects can be vaulted. The speed of vaults can be increased with the Verticality module, though avoiding vaults entirely is generally faster. Vaulting is initiated by moving towards whilst looking at a ledge, so it is possible to avoid vaulting a ledge by jumping towards it and then moving the camera away from the ledge, with the velocity maintained from the jump taking you past or [jump cancelling](#jump-cancels) onto the ledge. 
### 8.1 Vault Clipping <a name="vault-clipping"></a>
In some situations, vaulting can clip the player through other geometry.
### 8.2 Vault to jump <a name="vault-to-jump"></a>
After a vault animation [and other animations?] the player state is set to grounded state, which can be jumped from.

## 9 Grabbable Items <a name="grabbable-items"></a>
Some items can be picked up and thrown, dropped, or used to melee. When held, the item maintains some physical properties, like blocking incoming bullets. If the player falls too quickly, a held item will automatically be dropped.
### 9.1 Item Clipping <a name="item-clipping"></a>
When items are thrown or dropped, they can collide with the player to clip them through geometry. The specific setup (angle/position/velocity) required to clip varies depending on the level geometry and item type. Some geometry is too thick to be clipped. Items can often be easily dropped through walls. [Fps affects item clipping] [types of item clipping: throwing against one wall of a corner or an adjacent surface, dropping item whilst facing away from wall, wishdir being directed into the wall desired to clip]
### 9.2 Item Boosting <a name="item-boosting"></a>
When large enough items are thrown in midair, they can collide with the player to give a speed boost. The setup of such throws depend on the type of item used. Different setups can be used depending on whether height/distance/speed is desired. It is possible for items to become momentarily stuck to the player, giving a remarkable speed/distance of boost. Item boosts can be used to clear large gaps or simply gain speed. Items can be juggled by picking them up and boosting again repeatedly. Note that items may despawn when taken across loading zones. It is sometimes possible to catch and throw an item again in midair, for an additional boost. Thrown items can sometimes rebound off the NPC Psycho [and other objects?] and back into the player to give large boosts.

## 10 Fall Damage <a name="fall-damage"></a>
Fall damage increases proportional to velocity. Dying from fall damage is impossible with the Heavy Armour module. Sliding off slopes accelerates the player more quickly, allowing for increased fall damage. Landing on sloped terrain can negate fall damage.
### 10.1 Heavy fall clipping <a name="heavy-fall-clipping"></a>
Falls of a great enough velocity will trigger a hard fall animation, which can clip the player through collision.

<!-- gravity and fall damage, health, damage and critical health, weapons, objects and maps -->
