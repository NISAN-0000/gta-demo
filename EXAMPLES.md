# Code Examples & Extension Guide

## Creating Custom Weapons

### Example: Sniper Rifle

```cpp
// SniperRifle.h
#pragma once

#include "CoreMinimal.h"
#include "Weapons/GtaWeapon.h"
#include "SniperRifle.generated.h"

UCLASS()
class GTACLONE_API ASniperRifle : public AGtaWeapon
{
    GENERATED_BODY()

public:
    ASniperRifle();

    virtual void Fire() override;

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Sniper")
    float ZoomFOV = 30.0f;

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Sniper")
    bool bIsAiming = false;

    UFUNCTION(BlueprintCallable, Category = "Sniper")
    void AimIn();

    UFUNCTION(BlueprintCallable, Category = "Sniper")
    void AimOut();
};

// SniperRifle.cpp
#include "Weapons/SniperRifle.h"

ASniperRifle::ASniperRifle()
{
    WeaponType = EWeaponType::Rifle;
    Damage = 100.0f;
    FireRate = 2.0f;
    Range = 10000.0f;
    MagazineSize = 5;
}

void ASniperRifle::Fire()
{
    if (bIsAiming)
    {
        // Increased accuracy when aiming
        Super::Fire();
    }
}

void ASniperRifle::AimIn()
{
    bIsAiming = true;
    // TODO: Add camera zoom effect
}

void ASniperRifle::AimOut()
{
    bIsAiming = false;
    // TODO: Reset camera zoom
}
```

## Creating Custom Vehicles

### Example: Police Car

```cpp
// PoliceVehicle.h
#pragma once

#include "CoreMinimal.h"
#include "Vehicles/GtaVehicle.h"
#include "PoliceVehicle.generated.h"

UCLASS()
class GTACLONE_API APoliceVehicle : public AGtaVehicle
{
    GENERATED_BODY()

public:
    APoliceVehicle();

    virtual void BeginPlay() override;

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Police")
    bool bLightsOn = false;

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Police")
    bool bSirenOn = false;

    UFUNCTION(BlueprintCallable, Category = "Police")
    void ToggleLights();

    UFUNCTION(BlueprintCallable, Category = "Police")
    void ToggleSiren();

    UFUNCTION(BlueprintCallable, Category = "Police")
    void PursuitTarget(AActor* Target);

private:
    AActor* PursuedActor = nullptr;
    float PursuitSpeed = 250.0f;
};

// PoliceVehicle.cpp
#include "Vehicles/PoliceVehicle.h"

APoliceVehicle::APoliceVehicle()
{
    VehicleType = EVehicleType::Car;
    MaxSpeed = 250.0f;
    Acceleration = 4000.0f;
}

void APoliceVehicle::BeginPlay()
{
    Super::BeginPlay();
    // Set police vehicle appearance
}

void APoliceVehicle::ToggleLights()
{
    bLightsOn = !bLightsOn;
    // TODO: Add light effect
}

void APoliceVehicle::ToggleSiren()
{
    bSirenOn = !bSirenOn;
    // TODO: Add siren sound
}

void APoliceVehicle::PursuitTarget(AActor* Target)
{
    PursuedActor = Target;
    MaxSpeed = PursuitSpeed;
}
```

## Creating Custom Missions

### Example: Bank Robbery Mission

```cpp
// BankRobberyMission.h
#pragma once

#include "CoreMinimal.h"
#include "Missions/GtaMission.h"
#include "BankRobberyMission.generated.h"

class ABank;
class AGtaCharacter;

UCLASS()
class GTACLONE_API UBankRobberyMission : public UGtaMission
{
    GENERATED_BODY()

public:
    UBankRobberyMission();

    virtual void StartMission(AGtaCharacter* Player) override;
    virtual void CompleteMission(AGtaCharacter* Player) override;

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "BankRobbery")
    ABank* TargetBank;

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "BankRobbery")
    float RobberyAmount = 100000.0f;

    UFUNCTION(BlueprintCallable, Category = "BankRobbery")
    void RobBank();

    UFUNCTION(BlueprintCallable, Category = "BankRobbery")
    void EscapeBank(AGtaCharacter* Player);

private:
    bool bBankRobbed = false;
    float MissionStartTime = 0.0f;
};

// BankRobberyMission.cpp
#include "Missions/BankRobberyMission.h"
#include "Character/GtaCharacter.h"
#include "Engine/World.h"

UBankRobberyMission::UBankRobberyMission()
{
    MissionName = "Bank Heist";
    MissionDescription = "Rob the Fleeca Bank with the crew";
    MissionObjective = EMissionObjective::Steal;
    RewardAmount = 50000.0f;
    RewardRespect = 500;
}

void UBankRobberyMission::StartMission(AGtaCharacter* Player)
{
    Super::StartMission(Player);
    MissionStartTime = GetWorld()->GetTimeSeconds();
    // TODO: Trigger bank robbery cutscene
}

void UBankRobberyMission::RobBank()
{
    if (!bBankRobbed && TargetBank)
    {
        bBankRobbed = true;
        UpdateMissionProgress(50.0f);
        // TODO: Disable bank doors, trigger alarm
    }
}

void UBankRobberyMission::EscapeBank(AGtaCharacter* Player)
{
    if (bBankRobbed && Player)
    {
        CompleteMission(Player);
    }
}
```

## Working with the Crime System

### Example: Spawning Police

```cpp
void APoliceManager::OnCrimeCommitted(ECrimeType CrimeType, float BountyAmount)
{
    if (CrimeSystem->GetWantedLevel() >= 2)
    {
        // Spawn police vehicles
        SpawnPoliceUnits(CrimeSystem->GetWantedLevel());
    }

    if (CrimeSystem->GetWantedLevel() >= 4)
    {
        // Dispatch helicopter
        SpawnHelicopter();
    }
}

void APoliceManager::SpawnPoliceUnits(int32 WantedLevel)
{
    int32 NumUnits = WantedLevel * 2;
    
    for (int32 i = 0; i < NumUnits; ++i)
    {
        FVector SpawnLocation = GetRandomSpawnLocation();
        APoliceVehicle* PoliceVehicle = GetWorld()->SpawnActor<APoliceVehicle>(
            APoliceVehicle::StaticClass(),
            SpawnLocation,
            FRotator::ZeroRotator
        );
        
        if (PoliceVehicle && MainCharacter)
        {
            PoliceVehicle->PursuitTarget(MainCharacter);
        }
    }
}
```

## Creating Interactive Objects

### Example: ATM

```cpp
// ATM.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "ATM.generated.h"

class AGtaCharacter;

UCLASS()
class GTACLONE_API AATM : public AActor
{
    GENERATED_BODY()

public:
    AATM();

    virtual void BeginPlay() override;

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "ATM")
    float DepositAmount = 1000.0f;

    UFUNCTION(BlueprintCallable, Category = "ATM")
    void InteractWithATM(AGtaCharacter* Character);

    UFUNCTION(BlueprintCallable, Category = "ATM")
    void Rob(AGtaCharacter* Character);

private:
    int32 ATMBalance = 5000;
    bool bHasBeenRobbed = false;
};

// ATM.cpp
#include "Interactive/ATM.h"
#include "Character/GtaCharacter.h"

AATM::AATM()
{
    PrimaryActorTick.bCanEverTick = false;
    
    // Create mesh
    UStaticMeshComponent* Mesh = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Mesh"));
    RootComponent = Mesh;
}

void AATM::BeginPlay()
{
    Super::BeginPlay();
}

void AATM::InteractWithATM(AGtaCharacter* Character)
{
    if (Character)
    {
        Character->AddMoney(DepositAmount);
        // TODO: Show UI for deposit/withdraw
    }
}

void AATM::Rob(AGtaCharacter* Character)
{
    if (!bHasBeenRobbed && Character)
    {
        bHasBeenRobbed = true;
        Character->AddMoney(ATMBalance);
        ATMBalance = 0;
        
        // Trigger crime
        // TODO: Notify crime system
    }
}
```

## Game Mode Extensions

### Example: Adding a Money Pickup System

```cpp
void AGtaGameMode::SpawnMoneyPickups()
{
    for (int32 i = 0; i < 10; ++i)
    {
        FVector RandomLocation = GetRandomLocationInWorld();
        AMoneyPickup* Pickup = GetWorld()->SpawnActor<AMoneyPickup>(
            AMoneyPickup::StaticClass(),
            RandomLocation,
            FRotator::ZeroRotator
        );
        
        if (Pickup)
        {
            Pickup->SetAmount(FMath::RandRange(100, 1000));
        }
    }
}
```

## Listening to Delegates

### Example: HUD Widget

```cpp
void UGameHUD::NativeConstruct()
{
    Super::NativeConstruct();

    if (AGtaGameMode* GameMode = Cast<AGtaGameMode>(
        GetWorld()->GetAuthGameMode()))
    {
        if (AGtaCharacter* Player = GameMode->GetMainPlayer())
        {
            // Listen to health changes
            Player->OnHealthChanged.AddDynamic(this, &UGameHUD::UpdateHealth);

            // Listen to money changes
            Player->OnMoneyChanged.AddDynamic(this, &UGameHUD::UpdateMoney);
        }

        // Listen to wanted level changes
        if (GameMode->CrimeSystem)
        {
            GameMode->CrimeSystem->OnWantedLevelChanged.AddDynamic(
                this, &UGameHUD::UpdateWantedLevel);
        }
    }
}

void UGameHUD::UpdateHealth(float NewHealth, float MaxHealth)
{
    float HealthPercent = NewHealth / MaxHealth;
    HealthBar->SetPercent(HealthPercent);
}

void UGameHUD::UpdateMoney(float NewMoney, float TotalMoney)
{
    MoneyText->SetText(FText::FromString(
        FString::Printf(TEXT("$%d"), (int32)NewMoney)));
}

void UGameHUD::UpdateWantedLevel(int32 NewWantedLevel)
{
    WantedLevelWidget->SetWantedStars(NewWantedLevel);
}
```

## Performance Tips

### Efficient Line Traces for Weapons
```cpp
void AGtaWeapon::DealDamageOptimized()
{
    // Cache frequently accessed values
    FVector TraceStart = GetActorLocation();
    FVector TraceEnd = TraceStart + GetActorForwardVector() * Range;

    FHitResult HitResult;
    FCollisionQueryParams Params;
    Params.AddIgnoredActor(GetOwner());
    Params.bTraceComplex = false;  // Simpler collision = faster trace
    Params.bReturnPhysicalMaterial = false;

    if (GetWorld()->LineTraceSingleByChannel(HitResult, TraceStart, TraceEnd,
        ECC_Visibility, Params))
    {
        // Handle hit
    }
}
```

### Vehicle Physics Optimization
```cpp
void AGtaVehicle::UpdateMovementOptimized(float DeltaTime)
{
    // Only update if driver exists
    if (!CurrentDriver)
        return;

    // Cache physics values
    FVector Velocity = VehicleBody->GetComponentVelocity();
    float Speed = Velocity.Length();

    // Use cheaper operations first
    if (Speed < MaxSpeed * 0.1f)
        return;  // Skip expensive calculations at low speed

    // Then do complex calcs
    // ...
}
```

---

Use these examples as templates for your own systems!
