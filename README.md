# TaiwanCard

## Abstract Design of Games

High level requirements:

1. There exists a visual representation of the game progress that human can check. The visual representation might not be complete in that some part could be code.
2. The visual representation is defined by some abstract interfaces that must be implemented by the LLM.

Abstract entities:

- Card
- CardZone
- Player

... this is hard to design before any game implementations. Hence, we should first design some games and then summarize the common traits before come up with this abstract interface.

## Human-Designed Yu-Gi-Oh!-Inspired Card Game

### Principals

- The player going second has little thing to do except hand-trap.
- Monsters can only attack opponent's monster and the opponent. Why not loose this constraint?
- Monsters has significantly more strategies/variants/labels/attributes than traps and spells.
- Life point > 0 doesn't give much advantage though LP determine the outcome of the game.

### Core Economy

- Starting Funding: Each player begins with $10,000 in their Bank.

- Loss Condition: If your Bank ever reaches $0 or below, you lose the game.

- Paying Costs: Write costs as “Pay $1,000 × y.” If you can’t pay the full amount, you can’t start that action.

### Keywords & Notation

- FL (Funding Level): 0–12, printed on cards.
- IR (Influence Rank): 0–12, printed on cards.
- Supporter: A face-up Figure with printed IR you control. (Cards without printed IR aren’t eligible unless a card says otherwise.)
- Dead Zone: Your discard/boneyard.
- Silenced Zone: Your banished/exile zone.
- Orientation: Choose Center / Left / Right when a card enters the field.
- Empty slot check: If no empty slot is available, you can’t start a deployment that needs one.

1. Card Information

- Tactic card
  - labels
    - funding level, FL8, 🪙. FL0 to FL12
    - color:
      - black money — undeclared/illicit funds (tax-evasion, corruption, shadow economy).
      - white money — legally earned, declared income.
      - grey/gray money — borderline or unreported income (informal/“grey” economy), not necessarily from crime.
      - pink money — purchasing power of LGBTQ+ consumers (also “pink dollar/pound/yen”).
      - green money — environmentally focused finance/investment.
      - red money - communism party
    - effects:
      - definitions: series of actions executed
      - executing conditions: when placing on field, when activating by players or otherwise specified on card.
  - type
    - 普通：標準戰術 / Standard Tactic / 標準戦術
    - 速攻：速決戰術 / Quick Tactic / 速攻戦術
    - 場地：戰場 / Battleground / 戦場
    - 裝備：顧問團 / Advisory Suite / 顧問団
    - 儀式：集結號 / Call to Arms / 招集
- Figure card
  - labels
    - central attack
    - left attack
    - right attack
    - influence rank: IR0, 📣. IR0 to IR12
    - sex
    - race
    - age
    - nationality
    - effects:
      - definitions: series of actions executed
      - executing conditions: when deploying on field, when activating by players or otherwise specified on card.
  - type
    - political figure
    - media figure
    - business figure
    - normal figure
- Countermeasure card
  - labels
    - funding level, FL8, 🪙. FL0 to FL12
    - color
    - effects:
      - definitions: series of actions executed
      - executing conditions: when placing on field, when activating by players or otherwise specified on card.
  - type
    - Standard Countermeasure
    - Quick Intercept
    - Lockdown Net
    - Defensive Perimeter

2. Card Statuses

- Tactic card
  - Face-up
  - Face-down
- Figure card
  - Face-up orientations
    - Center: card head points straight at the opponent (↑).
    - Left: card head points toward the player’s left (<-).
    - Right: card head points toward the player's right (->).
  - Face-down orientations
    - Center: card head points straight at the opponent (↑).

3. Effects

- Effect Labels
  - Speed:
    - Definition: When a card’s effect is activated, its speed determines what time it can be activated and how it interacts with other effects.
    - Range
      - Speed 1:
        - Can be activated by players during their Main Phases.
        - Can take effect on phase events happens (e.g., drawing phase starts).
        - Cannot be activated in response to other effects.
      - Speed 2:
        - Can be activated or take effects during either player’s turn.
        - Can be activated or take effects in response to non-effect events, Speed 1 or Speed 2 effects.
      - Speed 3:
        - Can be activated or take effects during either player’s turn.
        - Can be activated or take effects in response to non-effect events, Speed 1, Speed 2, or Speed 3 effects.
      - Speed 4:
        - Can be activated or take effects during either player’s turn.
        - Can be activated or take effects in response to non-effect events, Speed 1, Speed 2, or Speed 3 effects.
  - Optionality
    - Mandatory
    - Optional
  - Duration
    - Permanent:
      - Starts when the effect resolves.
      - Lasts indefinitely depending on some condition.
      - Ends on non-phase events (e.g., when the card leaves the field).
    - Instant:
      - Changes the game state immediately when the effect resolves.
    - Temporary:
      - Starts when the effect resolves.
      - Ends on phase events (e.g., end of turn).
  - Constraints
    - preconditions
    - postconditions
  - Usage Limits
    - Once per turn for this card instance when it is visible to both players (e.g., field, dead zone, silenced zone).
    - Once per card name per turn.
    - Once per game.
- Effect Events

4. Player Started Actions

- Deployment
  - Objects: Figure Cards
  - Types
    - Secret Deployment
      - Timing: Your Main Phases
      - Source: Your Hand
      - Effect: Set a face-down Figure with printed IR = 0 in Center orientation.
      - Steps:
        1. Choose a Figure (IR 0) in your hand.
        2. Place it face-down on your field in Center orientation
    - Fundraising Deployment
      - Timing: Your Main Phases
      - Source: Your Hand
      - Target: A Figure Y in your hand with IR = y.
      - Cost: Send any number of Tactic/Countermeasure cards you control with total FL = x ≤ y to the Dead Zone.
      - Steps:
        1. Reveal Figure Y (IR = y) from your hand.
        2. Choose Tactic/Countermeasure cards you control with combined FL x ≤ y; send them to the Dead Zone.
        3. Deploy Y face-up to your field; choose orientation: Center/Left/Right.
    - Influence Deployment
      - Supporter (eligibility): face-up Figures with printed IR you control.
      - Timing: Your Main Phases
      - Source: Public Deck
      - Cost: Pay $1000 \* k, where k is the number of supporters you choose.
      - Steps:
        1. Reveal a Figure Y from your Public Deck (IR = y).
        2. Choose 1 to y eligible supporters you control (k = chosen count); overlay them under an empty slot you control (they become attached to that slot).
        3. Pay $1,000 × k. (If you can’t pay, this action can’t proceed.)
        4. Deploy Y face-up to that slot; choose Center / Left / Right.
      - Cleanup: If Y leaves the field, send all of its supporters to the Dead Zone, unless otherwise stated.
      - Freshness rule: A card can’t become a supporter the turn it entered your field.
      - While attached: Supporters are not on the field, don’t occupy slots, and can’t be targeted unless a card says otherwise.
    - Special Deployment
      - Timing/Source: As specified by the effect.
      - Target: As specified by the effect.
      - Resolution: Follow the card’s instructions. Unless stated, the Figure enters face-up and you choose Center / Left / Right.

- Placement
  - Objects: Tactic/Countermeasure Cards
  - Types
    - Face-down Placement
      - Timing: Your Main Phases
      - Source: Your Hand
      - Target: A Tactic/Countermeasure Card
      - Side Effects:
        - If you place a tactic card face-down, it cannot be activated or be used in a fundraising deployment at the same turn.

- Effects Activation
  - Objects: Tactic/Countermeasure/Figure Cards
  - Types:
    - Figure Card Effect Activation
    - Tactic Card Effect Activation
    - Countermeasure Card Effect Activation

- Phase Related
- Attack
- Make Choices
  - Objects: As specified by effects or rules.
  - Types:
    - Choose a number within a range.
    - Choose a card from a zone.
    - Choose a slot.
    - Choose a player.
    - etc.

5. Zones and Decks

- Hands
  - Unordered Zones
  - Capacity: non-negative integer
  - Visibility: private
  - Types:
    - Player's Hand
    - Opponent's Hand
- Fields
  - Partially Ordered Zone
  - Composition of each slot:
    - Case 1: Occupied by Figure card.
      - Top Card:
        - Capacity: 1 or 0
        - Face-up Center, Left, Right orientations. Face-down Center orientation.
        - Visibility:
          - Face-up: public
          - Face-down: inherited from the card's original visibility. E.g., if the card is from player's hand, then only player can see it. If the card is turned face-down from face-up, then both players can see it but it's information cannot be identified by card effects.
      - Supporters:
        - Capacity: non-negative integer.
        - Neither face-up nor face-down.
        - Visibility: public.
        - Unordered.
    - Case 2: Empty
      - Top Card:
        - Capacity: 0
      - Supporters: N/A
    - Case 3: Occupied by Tactic/Countermeasure card.
      - Top Card:
        - Capacity: 1 or 0
        - Face-up or Face-down
        - Visibility:
          - Face-up: public
          - Face-down: inherited from the card's original visibility.
        - Center orientation only.
      - Supporters: N/A
  - Use the (x, y) coordinates to represent each slot:
    - x ∈ {0, 1, 2, 3, 4}
    - y ∈ {0, 1, 2, 3, 4}
    - (0, 0) is the bottom-left corner from the player's perspective.
    - (4, 4) is the top-right corner from the player's perspective.
  - The player can use the bottom three rows (x, y) where y ∈ {0, 1, 2} and x ∈ {0, 1, 2, 3, 4}.
  - The opponent can use the top three rows (x, y) where y ∈ {2, 3, 4} and x ∈ {0, 1, 2, 3, 4}.
  - Notice that the middle row (y = 2) is controlled by both players meaning that both players can deploy cards to this row. Also, both players can pick cards from this row when declaring attacks.
  - Notice that each slot can be occupied by Figure card or Tactic/Countermeasure card.
  - Each player can only deploy/place cards to slots that they control.
- Private Decks
  - Contents: Any mix of Tactic, Countermeasure, and Non-public Figure cards.
  - Size: 40–60 cards.
  - Duplicates: Up to 3 copies per card name across your Private Deck + Public Deck combined.
  - Hidden/Public: Face-down, hidden to the opponent.
  - Shuffling: Shuffle at game start. Shuffle whenever instructed to “shuffle” or after any search unless the card says otherwise.
  - Start Hand: Draw 5 at the start of the game (before the first turn).
  - Mulligan: Once per game, in the end of your draw phase, you may pay $1000, shuffle your hand into your Private Deck and draw the same number(at most 5) of cards.
  - Empty-Draw rule: If you must draw and you have no more cards, you lose the game.

- Public Deck
  - Purpose: Source for Influence Deployment only.
  - Contents: 0-20 Public Figure cards, IR > 0.
  - Visibility: Face-up list; both players may see the entire Public Deck at all times
  - Ordered
  - Not a draw: Taking from the Public Deck is not a draw; it doesn’t trigger draw-based effects.
- Dead Zone
  - Visibility: Face-up, public; cards keep their identity and are in a single ordered pile (top = most recent).
  - Ownership: Cards go to their owner’s Dead Zone.
  - Supporters cleanup: If a Figure leaves the field, all its supporters go to the owner’s Dead Zone.
- Silenced Zone
  - Default visibility: Face-up, public.

6. Game Phases

- Turn Start Event
  - Draw Phase
    - Phase Start Event
    - Draw Action
    - Phase End Event
  - Main Phase 1
    - Phase Start Event
    - Players Effect Actions
    - Phase End Event
  - Attack Phase
    - Phase Start Event
    - Attack Actions
    - Phase End Event
  - Main Phase 2
    - Phase Start Event
    - Players Actions
    - Phase End Event
- Turn End Event

Note: The first turn of the whole game does not have a Draw Phase and an Attack Phase.

7. Attack

- Figures have three printed attack stats: central / left / right.
- A Figure’s orientation determines which stat it uses when it attacks.

Eligibility & Timing

- When: During your Attack Phase.

- Who: Each face-up Figure you control that began your turn under your control may declare one attack this turn.

- Freshness: Figures that entered your field this turn cannot attack.

Declare Attack

- Choose a face-up Figure you control that is eligible to attack.
- Choose a target:
  - If there exist a card controlled by the opponent, in relative vector r = (0, 1), (1, 0), (1, 1), (-1, 1), (-1, 0) to the attacking Figure, then you must choose one of below as the target:
    - The card on relative vector r.
    - The card itself.
    - The player who is declaring attack.
  - Otherwise, you must choose one of below as the target:
    - The card itself.
    - The player who is declaring the attack.
    - The opponent player.
- Attack Declaration Event triggered.
  - If the target leaves it's position or is no longer eligible to be attacked before the attack is resolved, choose a new target following the same rules above. If no valid target exists, the attack is cancelled.
  - If the attacking Figure is no longer eligible to attack before the attack is resolved, the attack is cancelled.
- The chance of attack for the attacking Figure is used up once.

Attack Resolution

- Attack Resolution Start Event triggered.

A. If the target is a Figure card:

1. Compare the attacking Figure's attack stat (based on its orientation) with the target Figure's attack stat (based on its orientation).
2. If attacking Figure's attack stat > target Figure's attack stat:
   - The target Figure is destroyed and sent to the Dead Zone.
   - The opponent player loses $difference.
3. If attacking Figure's attack stat < target Figure's attack stat:
   - The attacking Figure is destroyed and sent to the Dead Zone.
   - The attacking player loses $difference.
4. If attacking Figure's attack stat = target Figure's attack stat:
   - Both Figures are destroyed and sent to the Dead Zone.
   - No player loses any money.

- Figure Destruction Event triggered if any Figure is destroyed. The attacking figure destruction event is triggered before the target figure destruction event if both figures are destroyed.

B. If the target is a Tactic/Countermeasure card:

- The target card is destroyed and sent to the Dead Zone.

- Tactic/Countermeasure Destruction Event triggered.

C. If the target is a player:

- The target player loses attacking Figure's attack stat.

- Attack Resolution End Event triggered.

8. Effect
   An effect activation creates a Last In First Out (LIFO) effect stack. Each effect on the stack is resolved one at a time, starting with the most recently added effect. When an effect is resolved, it is removed from the stack.
   - Declaring an Effect
     1. Check Timing & Speed: Confirm the effect’s Speed is legal now (phase/response rules/costs).
     2. Pay Costs: Pay any costs required to activate the effect.
     3. Put on Stack: Place the effect on the effect stack.
     4. Effect Activation Event triggered.
     5. Respond: Both players may respond by activating other effects (putting them on the stack). The opponent may respond first and alternates until both players pass consecutively.
   - Resolving an Effect
     1. Take from Stack: Take the top effect from the stack.
     2. Resolve: Follow the effect’s instructions as much as possible. If you can’t complete the effect, do as much as you can.
     3. Record actions and achieved conditions for next potential new effect stacks.
     4. Repeat: If the stack isn’t empty, go back to step 1.
   - Open a window with recorded actions and achieved conditions for both players to activate effects (new stacks). The opponent may respond first and alternates until both players pass consecutively.

- projects
- game - ts library for core game related logic
- game-cli - ts library for CLI wrapper of the game
- backend - colyseus websocket server
- frontend - react/phaser app
