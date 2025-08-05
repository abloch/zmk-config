# ZMK Sofle Keyboard Configuration Project

## Project Overview
This project ports a Vial keymap configuration to ZMK format for a Sofle split keyboard. The goal is to maintain identical functionality between the original Vial layout and the ZMK implementation while accounting for physical keyboard differences.

## Key Files and Structure
- `config/sofle.keymap` - Main ZMK configuration file
- `screensots/akiva.vil` - Vial export file containing complete layout configuration
- `screensots/0.png` through `4.png` - Screenshots of Vial layers for visual reference
- `sofle.svg` - Keyboard layout diagram

## Critical Layout Understanding

### Physical Keyboard Differences
- **ZMK keyboard**: Has extra keys (top row and outer thumb keys) that should NOT be used
- **Vial keyboard**: Mini keyboard with supplementary sub-switches that don't exist on ZMK board
- **Top row policy**: Always use `&bootloader` at positions 0 and 12, `&none` everywhere else
- **Unused positions**: Fill with `&none` rather than leaving empty or using `&trans`

### Vial File Layout Format (CRITICAL)
**The right keyboard in the akiva.vil file is REVERSED (right-to-left)**
- Left keyboard: Normal left-to-right order
- Right keyboard: REVERSED right-to-left order
- This is the most common source of errors - always account for this reversal

### Layer Structure
The keyboard has 6 layers (0-5):
- **Layer 0**: Base layer (QWERTY with modifications)
- **Layer 1**: Symbols layer
- **Layer 2**: Navigation layer  
- **Layer 3**: Numbers layer
- **Layer 4**: Function layer
- **Layer 5**: Transparent layer (all `&trans`)

## Configuration Rules and Standards

### ZMK Documentation Reference
**IMPORTANT**: Always use the official ZMK documentation at https://zmk.dev/docs/ for any questions about ZMK syntax, behaviors, or configuration specifics. When in doubt about any ZMK feature, search the documentation first before making assumptions.

Key documentation sections:
- Keymaps: https://zmk.dev/docs/keymaps
- Behaviors: https://zmk.dev/docs/keymaps/behaviors  
- Combos: https://zmk.dev/docs/keymaps/combos
- Tap-Dance: https://zmk.dev/docs/keymaps/behaviors/tap-dance
- Macros: https://zmk.dev/docs/keymaps/behaviors/macros

### Keymap Formatting Requirements
1. **Column Alignment**: Always align all columns for maximum readability
2. **ASCII Diagrams**: Update layer diagrams whenever bindings change
3. **Consistent Spacing**: Use proper spacing between key definitions
4. **Bootloader Placement**: Always `&bootloader` at positions 0 and 12 of top row

### ZMK Syntax Standards
- Use `&kp` for key presses
- Use `&to X` for layer switching (not `TO(X)`)
- Use `&lt X KEY` for layer-tap behaviors
- Use `&td X` for tap dance references
- Use `&mkp` for mouse key presses
- Use `&none` for unused positions
- Use `&trans` only for pass-through behaviors

### Macro Definitions
All macros (M0-M20, arc1, arc2, etc.) are defined in the macros section. Key mappings:
- Vial `M0` → ZMK `&m0`
- Vial `M1` → ZMK `&m1`
- Continue pattern for all macros

### Tap Dance Behaviors
Defined tap dances:
- `td0`: GUI behaviors
- `td2`: Shift behaviors  
- `td3`: Alt behaviors
- `td10`: L key with macro on triple tap

## Layout Verification Process

### Step-by-Step Porting
1. **Read Vial Screenshots**: Understand visual layout of each layer
2. **Parse akiva.vil**: Extract exact key mappings (remember right side reversal)
3. **Map to ZMK Syntax**: Convert Vial codes to ZMK equivalents
4. **Align Columns**: Ensure readability
5. **Update ASCII Diagrams**: Keep visual references current
6. **Verify Build**: Check for syntax errors

### Common Vial → ZMK Mappings
- `KC_` prefix → `&kp` behavior
- `TO(X)` → `&to X`
- `LT(X, KEY)` → `&lt X KEY`
- `KC_NO` → `&none`
- `KC_TRNS` → `&trans`
- `LCTL(KC_X)` → `&kp LC(X)`
- `LGUI(KC_X)` → `&kp LG(X)`
- `LALT(KC_X)` → `&kp LA(X)`
- `LSFT(KC_X)` → `&kp LS(X)`

### Thumb Key Patterns
Each layer has specific thumb key configurations:
- **Layer 0**: `TD0`, `SP2`, `TD2` (left) | `CTL`, `EN1`, `TD3` (right)
- **Layer 1**: `GUI`, `SPC`, `SHF` (left) | `CTL`, `RET`, `ALT` (right)
- **Layer 2**: `GUI`, `SPC`, `SHF` (left) | `ALT`, `RET`, `CTL` (right)
- **Layer 3**: `GUI`, `SPC`, `SHF` (left) | `DOT`, `0`, `ALT` (right)
- **Layer 4**: `GUI`, `none`, `CAP` (left) | `CTL`, `none`, `ALT` (right)

## Quality Assurance Checklist

### Before Completing Any Layer
- [ ] Top row has bootloaders at positions 0 and 12, `&none` elsewhere
- [ ] All columns are properly aligned
- [ ] ASCII diagram matches actual bindings
- [ ] Right keyboard mappings account for Vial file reversal
- [ ] All referenced macros and behaviors are defined
- [ ] Thumb keys match the specified pattern for that layer
- [ ] No syntax errors (proper `&` prefixes, valid key codes)

### Build Verification
- Run `zmk build` to check for devicetree errors
- Verify no duplicate macro definitions
- Confirm all referenced behaviors exist
- Check for proper key code standards (ESC not ESCAPE, etc.)

## Important Reminders

### Critical Success Factors
1. **ALWAYS** remember right keyboard reversal in Vial file
2. **ALWAYS** align columns after making changes
3. **ALWAYS** update ASCII diagrams when changing bindings
4. **ALWAYS** use `&none` for unused positions, not empty or `&trans`
5. **ALWAYS** place bootloaders at positions 0 and 12 of top row

### Common Pitfalls to Avoid
- Forgetting right keyboard is reversed in Vial file
- Not aligning columns after edits
- Using wrong key codes (old vs new ZMK standards)
- Leaving ASCII diagrams outdated
- Misplacing thumb keys
- Using `&trans` where `&none` is appropriate

## Development Workflow
1. Make changes to specific layer
2. Align all columns in that layer
3. Update ASCII diagram for that layer
4. Verify against Vial file (accounting for reversal)
5. Test build for syntax errors
6. Commit changes with clear message

This configuration represents a complete port from Vial to ZMK while maintaining identical functionality and improving readability through proper formatting and documentation.