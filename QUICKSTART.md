# GTA Clone - Quick Start Guide

## 5-Minute Setup

### Step 1: Generate Visual Studio Project
```cmd
# In Windows Explorer, right-click GtaClone.uproject
# → Generate Visual Studio project files
```

### Step 2: Build in Visual Studio
```cmd
# Open GtaClone.sln in Visual Studio 2022
# Build → Build Solution
# Wait for compilation...
```

### Step 3: Open in Unreal Editor
```cmd
# Method 1: Double-click GtaClone.uproject
# Method 2: Right-click → Open with Unreal Engine 5.3+
```

### Step 4: Create Level
- Content Browser → Create New → Level → Blank Level
- Save as "MainGame"

### Step 5: Set Game Mode
- Project Settings → Maps & Modes
- Default GameMode → `AGtaGameMode`
- Default Player Pawn Class → Create Blueprint of `AGtaCharacter`

### Step 6: Create Character Blueprint
1. Content Browser → Create Blueprint
2. Parent Class → `AGtaCharacter`
3. Add a Skeletal Mesh in Details
4. Set this blueprint as Default Pawn Class

### Step 7: Set Up Input
1. Project Settings → Input
2. Create Input Actions:
   - `IA_Move` (Value Type: Axis2D)
   - `IA_Look` (Value Type: Axis2D)
   - `IA_Fire` (Value Type: Digital)
   - `IA_Sprint` (Value Type: Digital)

3. Create Input Mapping Context `IMC_Default`
4. Map keys in the context
5. Assign in Character Blueprint Details:
   - Default Mapping Context → `IMC_Default`
   - Move Action → `IA_Move`
   - Look Action → `IA_Look`
   - Fire Action → `IA_Fire`
   - Sprint Action → `IA_Sprint`

### Step 8: Play!
- Click "Play" in the editor
- Use WASD to move, Mouse to look, Click to shoot

## Common Tasks

### Add a Vehicle to the Level
1. Create Blueprint from `AGtaVehicle`
2. Add a Static/Skeletal Mesh
3. Drag into level
4. Press 'F' to enter (set up interaction later)

### Add Weapon
1. Create Blueprint from `AGtaWeapon`
2. Add mesh component
3. Adjust damage, fire rate, ammo
4. Assign to character in code

### Create a Mission
1. Create Blueprint from `UGtaMission`
2. Set:
   - Mission Name
   - Objective Type
   - Reward Amount
3. Implement logic in Event Graphs

## Keyboard Controls (Default)
| Control | Input |
|---------|-------|
| Move | WASD |
| Look Around | Mouse |
| Sprint | Shift |
| Fire | Left Click |
| Reload | R |
| Handbrake (Vehicle) | Ctrl |

## Next Steps
1. Add skeletal meshes for character
2. Add vehicle meshes (cars, motorcycles)
3. Create weapon animations
4. Build a city level with streets and buildings
5. Implement NPC system
6. Add sound effects
7. Create UI widgets for HUD

## Troubleshooting

**Compilation Error?**
- Check UE version is 5.3+
- Ensure Enhanced Input plugin is enabled
- Verify all #include paths are correct

**Character won't move?**
- Check Input Mapping Context is assigned
- Verify input actions are created
- Test input in Settings → Input

**Game crashes on play?**
- Check Default Game Mode is set
- Verify Default Pawn Class exists in Content
- Check console for error messages

**Can't see character?**
- Verify Skeletal Mesh is assigned to blueprint
- Check camera boom arm length (should be 400+)
- Verify level has proper lighting

## Resources
- [UE5.3 Documentation](https://docs.unrealengine.com/5.3)
- [Enhanced Input System](https://docs.unrealengine.com/5.0/en-US/enhanced-input-user-guide-in-unreal-engine/)
- Framework header files in `Source/GtaClone/Public/`

---

Once you complete these steps, you'll have a working GTA-like game foundation!

Check README.md for detailed system documentation.
