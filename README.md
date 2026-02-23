# GTA Clone - Unreal Engine 5 Game Framework

A comprehensive open-world action game framework inspired by GTA 5, built with Unreal Engine 5 and C++.

## Features

### Core Systems Implemented

- **Player Character System** (`AGtaCharacter`)
  - Third-person controller with Enhanced Input System
  - Movement (walk/sprint)
  - Health system with inventory
  - Money/stats tracking
  - Event delegates for UI integration

- **Combat System** (`AGtaWeapon`)
  - Multiple weapon types (Pistol, Rifle, Shotgun, Melee)
  - Damage system with line traces
  - Ammunition management
  - Reload mechanics
  - Fire rate control

- **Vehicle System** (`AGtaVehicle`)
  - Multiple vehicle types (Car, Truck, Motorcycle, Helicopter)
  - Physics-based driving mechanics
  - Fuel consumption system
  - Vehicle damage and destruction
  - Driver entry/exit system

- **Crime System** (`UGtaCrimeSystem`)
  - Wanted level tracking (0-5 stars)
  - Dynamic bounty system
  - Crime event tracking
  - Police evasion timer
  - Automatic wanted level decrease

- **Mission System** (`UGtaMission`)
  - Mission framework with objectives
  - Progress tracking
  - Reward system (money + respect)
  - Multiple mission types:
    - Assassination
    - Vehicle Theft
    - Delivery
    - Escape
    - Bodyguard
    - Racing

- **Game Mode** (`AGtaGameMode`)
  - Game state management
  - Day/night cycle system
  - Time scaling
  - Player initialization
  - Game pause/resume
  - Save/Load framework (ready for implementation)

## Project Structure

```
GtaClone/
├── Source/
│   ├── GtaClone/
│   │   ├── Public/
│   │   │   ├── Character/
│   │   │   │   └── GtaCharacter.h
│   │   │   ├── Weapons/
│   │   │   │   └── GtaWeapon.h
│   │   │   ├── Vehicles/
│   │   │   │   └── GtaVehicle.h
│   │   │   ├── Crime/
│   │   │   │   └── GtaCrimeSystem.h
│   │   │   ├── Missions/
│   │   │   │   └── GtaMission.h
│   │   │   ├── GtaGameMode.h
│   │   │   └── GtaClone.h
│   │   ├── Private/
│   │   │   ├── Character/
│   │   │   │   └── GtaCharacter.cpp
│   │   │   ├── Weapons/
│   │   │   │   └── GtaWeapon.cpp
│   │   │   ├── Vehicles/
│   │   │   │   └── GtaVehicle.cpp
│   │   │   ├── Crime/
│   │   │   │   └── GtaCrimeSystem.cpp
│   │   │   ├── Missions/
│   │   │   │   └── GtaMission.cpp
│   │   │   ├── GtaGameMode.cpp
│   │   │   └── GtaClone.cpp
│   │   └── GtaClone.Build.cs
│   ├── GtaCloneTarget.cs
│   └── GtaCloneEditor.Target.cs
├── GtaClone.uproject
└── README.md
```

## Setup Instructions

### Prerequisites

- Unreal Engine 5.3 or later
- Visual Studio 2022 (for Windows development)
- C++ knowledge

### Installation

1. **Clone/Download** this project to your desired location
2. **Right-click** `GtaClone.uproject` → "Generate Visual Studio project files"
3. **Open** `GtaClone.sln` in Visual Studio 2022
4. **Build** the solution (Build → Build Solution)
5. **Open** the project in Unreal Engine:
   - Right-click `GtaClone.uproject`
   - Select "Open with Unreal Engine"

### First Time Setup in Unreal Editor

1. Create a new level or open the default level
2. Set `AGtaGameMode` as the default GameMode in Project Settings
3. Create a character blueprint based on `AGtaCharacter`
4. Create vehicle blueprints based on `AGtaVehicle`
5. Create weapon blueprints based on `AGtaWeapon`
6. Set up input mappings in Project Settings → Input

## Input Setup (Enhanced Input System)

The game uses Unreal Engine's Enhanced Input System. You'll need to set up:

### Input Actions
- `IA_Move` - WASD/Analog Stick
- `IA_Look` - Mouse/Right Analog Stick
- `IA_Fire` - Left Mouse Button / Right Trigger
- `IA_Sprint` - Shift / Left Trigger
- `IA_Brake` - Space / X Button
- `IA_Handbrake` - Ctrl / Square Button

### Input Mapping Context
- `IMC_Default` - Maps all actions to actual keys

**Note**: Create these input assets in the editor and assign them in `AGtaCharacter`'s details panel.

## Game Configuration

Edit values in the Character/Vehicle/Crime blueprints:

### Character Settings (AGtaCharacter)
- `WalkSpeed` - Default movement speed (600.0)
- `SprintSpeed` - Sprint movement speed (1200.0)
- `MaxHealth` - Maximum health points (100.0)

### Vehicle Settings (AGtaVehicle)
- `MaxSpeed` - Maximum vehicle speed
- `Acceleration` - How quickly vehicle accelerates
- `TurningRate` - Vehicle turning responsiveness
- `MaxFuel` - Maximum fuel capacity
- `FuelConsumption` - Fuel usage rate

### Crime System Settings (UGtaCrimeSystem)
- `MaxWantedLevel` - Maximum wanted level (0-5)
- `BountyMultiplier` - Bounty per wanted level

## Quick Start Guide

1. **Create a Game Level**:
   - New Level → Create base meshes for streets/buildings
   - Place multiple `AGtaVehicle` blueprints around the world
   - Place health pickups and ammo crates

2. **Create Missions**:
   - Create blueprints derived from `UGtaMission`
   - Set mission objectives and rewards
   - Add to a mission manager system

3. **Implement UI**:
   - Create HUD widget for health, money, wanted level
   - Bind to the delegates in `AGtaCharacter` and `UGtaCrimeSystem`
   - Add mission objective widget

4. **Polish and Expand**:
   - Add animations for characters and vehicles
   - Create dialog system for NPCs
   - Implement AI for police and civilians
   - Add effects and sounds

## Extending the Framework

### Adding New Weapons

```cpp
// Create a new class derived from AGtaWeapon
class ATaserGun : public AGtaWeapon
{
    // Override Fire() to implement custom behavior
};
```

### Creating New Missions

```cpp
// Create a new class derived from UGtaMission
UCLASS()
class UAssassinationMission : public UGtaMission
{
    UPROPERTY(EditAnywhere)
    AActor* TargetActor;
    
    virtual void StartMission(AGtaCharacter* Player) override;
    virtual void CompleteMission(AGtaCharacter* Player) override;
};
```

### Adding New Vehicle Types

```cpp
// Create a new class derived from AGtaVehicle
UCLASS()
class ATank : public AGtaVehicle
{
    UPROPERTY(EditAnywhere)
    float ArmorValue = 500.0f;
    
    virtual void DamageVehicle(float DamageAmount) override;
    virtual void Fire();
};
```

## Key Delegates and Events

### AGtaCharacter
- `OnHealthChanged` - Broadcasts when character takes damage
- `OnMoneyChanged` - Broadcasts when character gains/loses money

### UGtaCrimeSystem
- `OnWantedLevelChanged` - Broadcasts when wanted level changes
- `OnCrimeCommitted` - Broadcasts when crime is committed

### UGtaMission
- `OnMissionStarted` - Mission begins
- `OnMissionCompleted` - Mission completed successfully
- `OnMissionFailed` - Mission failed

### AGtaGameMode
- `OnGameInitialized` - Game systems initialized
- `OnGameStateChanged` - Game state changes (Paused, Resumed, etc.)

## Performance Optimization Tips

1. **LOD System**: Use Unreal's Level of Detail system for distant buildings/vehicles
2. **Culling**: Enable occlusion culling in your level
3. **Physics**: Use simpler collision for distant vehicles
4. **AI**: Implement hierarchical AI - only update nearby NPCs frequently
5. **Streaming**: Use world partition for large open worlds

## Known Limitations & TODO

- [ ] Save/Load system implementation
- [ ] NPC AI system
- [ ] Dialog system
- [ ] Animation system
- [ ] Sound system
- [ ] Advanced police behavior
- [ ] Gang system
- [ ] Property system
- [ ] Business system
- [ ] Multiplayer support

## Troubleshooting

### Build Issues
- **Compilation Errors**: Ensure you're using UE 5.3+
- **Missing Dependencies**: Verify Enhanced Input plugin is enabled
- **Include Path Errors**: Check that all header paths are correct

### Gameplay Issues
- **Character Not Moving**: Verify input actions are set up correctly
- **Weapons Not Firing**: Check weapon owner is set properly
- **Vehicle Physics Unstable**: Increase mass or reduce acceleration

## Resources

- [Unreal Engine Documentation](https://docs.unrealengine.com)
- [Enhanced Input System Guide](https://docs.unrealengine.com/5.0/en-US/enhanced-input-user-guide-in-unreal-engine/)
- [C++ API Reference](https://docs.unrealengine.com/5.0/en-US/API/)

## Contributing

Feel free to expand this framework with:
- New game mechanics
- Additional vehicle types
- More mission variety
- Improved AI systems
- Visual enhancements

## License

This is an educational framework. Commercial use should respect GTA's intellectual property.

## Support

For issues or questions:
1. Check the Unreal Engine documentation
2. Search Unreal Engine forums
3. Review the code comments for implementation details

---

Happy Game Development! 🎮
