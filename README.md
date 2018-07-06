FCNPC - Fully Controllable NPC
==============================
[![GitHub version](https://badge.fury.io/gh/ziggi%2FFCNPC.svg)](https://badge.fury.io/gh/ziggi%2FFCNPC) [![Build Status](https://travis-ci.org/ziggi/FCNPC.svg?branch=master)](https://travis-ci.org/ziggi/FCNPC) [![Build status](https://ci.appveyor.com/api/projects/status/e4cbhqdflr97arti?svg=true)](https://ci.appveyor.com/project/ziggi/fcnpc)

Fully Controllable NPC (FCNPC) is a plugin for SA-MP servers which adds a lot of capabilities to the existing standard NPCs.

The latest version can be found in the [releases section](../../releases).

This is a fork of the [original repository](https://github.com/OrMisicL/FCNPC) by [OrMisicL](http://forum.sa-mp.com/member.php?u=197901).

If you encounter a bug or a crash, create an issue in the [issues section](../../issues) with your **crashlog** and your **Pawn script**.

For more elaborate discussions see the [forum thread](http://forum.sa-mp.com/showthread.php?t=428066), or the [Russian forum thread](http://forum.sa-mp.com/showthread.php?t=602965).

See the [wiki](../../wiki) for documentation.

MapAndreas and ColAndreas support
---------------------------------
FCNPC supports working with [MapAndreas](http://forum.sa-mp.com/showpost.php?p=3130004&postcount=153) or [ColAndreas](http://forum.sa-mp.com/showthread.php?t=586068), for better NPC height positioning. Just add these plugins before FCNPC on the `plugins` line in the `server.cfg` file.

Sources
-------
To download the sources, use the following git command:
```bash
git clone --recursive https://github.com/ziggi/FCNPC.git
```
Note the use of the `--recursive` argument, because this repository contains submodules.

Building
--------
To build the project you can use [Visual Studio](https://www.visualstudio.com/). To generate the project you can use [CMake](https://cmake.org/).<br />
On Windows, execute the following commands. This example is for Visual Studio 15 2017:
```bash
cd FCNPC
mkdir build
cd build
cmake .. -G "Visual Studio 15 2017"
```

On Linux, execute the following commands:
```bash
cd FCNPC
mkdir build
cd build
cmake ..
make
```

Special thanks
--------------
- **SA-MP Team:** SA-MP
- **OrMisicL:** FCNPC creator
- **Zeex:** Subhook library
- **kurta999:** YSF plugin
- **Lorenc_, kurta999, Neutralneu, therainycat, Freaksken, karimcambridge:** Contributors
- **urShadow, Incognito:** Code samples in their plugins
- **Freaksken:** Creating and updating the wiki
- **Whole SA-MP community:** testing

Constants
---------
```Pawn
#define FCNPC_INCLUDE_VERSION		The current FCNPC include version.

#define FCNPC_MOVE_TYPE_AUTO		(-1)
#define FCNPC_MOVE_TYPE_WALK		(0)
#define FCNPC_MOVE_TYPE_RUN		(1)
#define FCNPC_MOVE_TYPE_SPRINT		(2)
#define FCNPC_MOVE_TYPE_DRIVE		(3)

#define FCNPC_MOVE_MODE_AUTO		(-1)
#define FCNPC_MOVE_MODE_NONE		(0)
#define FCNPC_MOVE_MODE_MAPANDREAS	(1)

#define FCNPC_MOVE_SPEED_AUTO		(-1.0)
#define FCNPC_MOVE_SPEED_WALK		(0.1552086)
#define FCNPC_MOVE_SPEED_RUN		(0.56444)
#define FCNPC_MOVE_SPEED_SPRINT		(0.926784)

#define FCNPC_MAX_NODES			(64)

#define FCNPC_INVALID_MOVEPATH_ID	(-1)
#define FCNPC_INVALID_RECORD_ID		(-1)

#undef FCNPC_DISABLE_VERSION_CHECK
```

Callbacks
---------
```Pawn
forward FCNPC_OnCreate(npcid);
forward FCNPC_OnDestroy(npcid);
forward FCNPC_OnSpawn(npcid);
forward FCNPC_OnRespawn(npcid);
forward FCNPC_OnDeath(npcid, killerid, reason);
forward FCNPC_OnUpdate(npcid);

forward FCNPC_OnTakeDamage(npcid, issuerid, weaponid, bodypart, Float:amount);
forward FCNPC_OnGiveDamage(npcid, damagedid, weaponid, bodypart, Float:amount);

forward FCNPC_OnReachDestination(npcid);

forward FCNPC_OnWeaponShot(npcid, weaponid, hittype, hitid, Float:fX, Float:fY, Float:fZ);
forward FCNPC_OnWeaponStateChange(npcid, weapon_state);

forward FCNPC_OnStreamIn(npcid, forplayerid);
forward FCNPC_OnStreamOut(npcid, forplayerid);

forward FCNPC_OnVehicleEntryComplete(npcid, vehicleid, seatid);
forward FCNPC_OnVehicleExitComplete(npcid);
forward FCNPC_OnVehicleTakeDamage(npcid, issuerid, vehicleid, weaponid, Float:fX, Float:fY, Float:fZ);

forward FCNPC_OnFinishPlayback(npcid);

forward FCNPC_OnFinishNode(npcid, nodeid);
forward FCNPC_OnFinishNodePoint(npcid, nodeid, pointid);
forward FCNPC_OnChangeNode(npcid, newnodeid, oldnodeid);

forward FCNPC_OnFinishMovePath(npcid, pathid);
forward FCNPC_OnFinishMovePathPoint(npcid, pathid, pointid);

forward FCNPC_OnChangeHeightPos(npcid, Float:newz, Float:oldz); // disabled by default, see FCNPC_SetMinHeightPosCall
```

Natives
-------
```Pawn
native FCNPC_GetPluginVersion(version[], const size = sizeof(version));
native FCNPC_SetUpdateRate(rate);
native FCNPC_GetUpdateRate();
native FCNPC_SetTickRate(rate);
native FCNPC_GetTickRate();
native FCNPC_IsMoveModeEnabled(mode);
native FCNPC_ToggleMoveModeEnabled(mode, bool:toggle);
native FCNPC_ToggleCrashLogCreation(bool:toggle);
native FCNPC_GetCrashLogCreation();

native FCNPC_Create(name[]);
native FCNPC_Destroy(npcid);
native FCNPC_Spawn(npcid, skinid, Float:x, Float:y, Float:z);
native FCNPC_Respawn(npcid);
native FCNPC_IsSpawned(npcid);
native FCNPC_Kill(npcid);
native FCNPC_IsDead(npcid);
native FCNPC_IsValid(npcid);
native FCNPC_IsStreamedIn(npcid, forplayerid);
native FCNPC_IsStreamedForAnyone(npcid);
native FCNPC_GetValidArray(npcs[], const size = sizeof(npcs));

native FCNPC_SetPosition(npcid, Float:x, Float:y, Float:z);
native FCNPC_GivePosition(npcid, Float:x, Float:y, Float:z);
native FCNPC_GetPosition(npcid, &Float:x, &Float:y, &Float:z);
native FCNPC_SetAngle(npcid, Float:angle);
native Float:FCNPC_GiveAngle(npcid, Float:angle);
native FCNPC_SetAngleToPos(npcid, Float:x, Float:y);
native FCNPC_SetAngleToPlayer(npcid, playerid);
native Float:FCNPC_GetAngle(npcid);
native FCNPC_SetQuaternion(npcid, Float:w, Float:x, Float:y, Float:z);
native FCNPC_GiveQuaternion(npcid, Float:w, Float:x, Float:y, Float:z);
native FCNPC_GetQuaternion(npcid, &Float:w, &Float:x, &Float:y, &Float:z);
native FCNPC_SetVelocity(npcid, Float:x, Float:y, Float:z, bool:update_pos = false);
native FCNPC_GiveVelocity(npcid, Float:x, Float:y, Float:z, bool:update_pos = false);
native FCNPC_GetVelocity(npcid, &Float:x, &Float:y, &Float:z);
native FCNPC_SetSpeed(npcid, Float:speed);
native Float:FCNPC_GetSpeed(npcid);
native FCNPC_SetInterior(npcid, interiorid);
native FCNPC_GetInterior(npcid);
native FCNPC_SetVirtualWorld(npcid, worldid);
native FCNPC_GetVirtualWorld(npcid);

native FCNPC_SetHealth(npcid, Float:health);
native Float:FCNPC_GiveHealth(npcid, Float:health);
native Float:FCNPC_GetHealth(npcid);
native FCNPC_SetArmour(npcid, Float:armour);
native Float:FCNPC_GiveArmour(npcid, Float:armour);
native Float:FCNPC_GetArmour(npcid);

native FCNPC_SetInvulnerable(npcid, bool:invulnerable = true);
native FCNPC_IsInvulnerable(npcid);

native FCNPC_SetSkin(npcid, skinid);
native FCNPC_GetSkin(npcid);
#if SAMP_03DL
	native FCNPC_GetCustomSkin(npcid);
#endif

native FCNPC_SetWeapon(npcid, weaponid);
native FCNPC_GetWeapon(npcid);
native FCNPC_SetAmmo(npcid, ammo);
native FCNPC_GiveAmmo(npcid, ammo);
native FCNPC_GetAmmo(npcid);
native FCNPC_SetAmmoInClip(npcid, ammo);
native FCNPC_GiveAmmoInClip(npcid, ammo);
native FCNPC_GetAmmoInClip(npcid);
native FCNPC_SetWeaponSkillLevel(npcid, skill, level);
native FCNPC_GiveWeaponSkillLevel(npcid, skill, level);
native FCNPC_GetWeaponSkillLevel(npcid, skill);
native FCNPC_SetWeaponState(npcid, weaponstate);
native FCNPC_GetWeaponState(npcid);

native FCNPC_SetWeaponReloadTime(npcid, weaponid, time);
native FCNPC_GetWeaponReloadTime(npcid, weaponid);
native FCNPC_GetWeaponActualReloadTime(npcid, weaponid);
native FCNPC_SetWeaponShootTime(npcid, weaponid, time);
native FCNPC_GetWeaponShootTime(npcid, weaponid);
native FCNPC_SetWeaponClipSize(npcid, weaponid, size);
native FCNPC_GetWeaponClipSize(npcid, weaponid);
native FCNPC_GetWeaponActualClipSize(npcid, weaponid);
native FCNPC_SetWeaponAccuracy(npcid, weaponid, Float:accuracy);
native Float:FCNPC_GetWeaponAccuracy(npcid, weaponid);
native FCNPC_SetWeaponInfo(npcid, weaponid, reload_time = -1, shoot_time = -1, clip_size = -1, Float:accuracy = 1.0);
native FCNPC_GetWeaponInfo(npcid, weaponid, &reload_time = -1, &shoot_time = -1, &clip_size = -1, &Float:accuracy = 1.0);
native FCNPC_SetWeaponDefaultInfo(weaponid, reload_time = -1, shoot_time = -1, clip_size = -1, Float:accuracy = 1.0);
native FCNPC_GetWeaponDefaultInfo(weaponid, &reload_time = -1, &shoot_time = -1, &clip_size = -1, &Float:accuracy = 1.0);

native FCNPC_SetKeys(npcid, ud_analog, lr_analog, keys);
native FCNPC_GetKeys(npcid, &ud_analog, &lr_analog, &keys);

native FCNPC_SetSpecialAction(npcid, actionid);
native FCNPC_GetSpecialAction(npcid);

native FCNPC_SetAnimation(npcid, animationid, Float:fDelta = 4.1, loop = 0, lockx = 1, locky = 1, freeze = 0, time = 1);
native FCNPC_SetAnimationByName(npcid, name[], Float:fDelta = 4.1, loop = 0, lockx = 1, locky = 1, freeze = 0, time = 1);
native FCNPC_ResetAnimation(npcid);
native FCNPC_GetAnimation(npcid, &animationid = 0, &Float:fDelta = 4.1, &loop = 0, &lockx = 1, &locky = 1, &freeze = 0, &time = 1);
native FCNPC_ApplyAnimation(npcid, animlib[], animname[], Float:fDelta = 4.1, loop = 0, lockx = 1, locky = 1, freeze = 0, time = 1);
native FCNPC_ClearAnimations(npcid);

native FCNPC_SetFightingStyle(npcid, style);
native FCNPC_GetFightingStyle(npcid);

native FCNPC_ToggleReloading(npcid, bool:toggle);
native FCNPC_HasReloading(npcid);
native FCNPC_ToggleInfiniteAmmo(npcid, bool:toggle);
native FCNPC_HasInfiniteAmmo(npcid);

native FCNPC_GoTo(npcid, Float:x, Float:y, Float:z, type = FCNPC_MOVE_TYPE_AUTO, Float:speed = FCNPC_MOVE_SPEED_AUTO, mode = FCNPC_MOVE_MODE_AUTO, Float:radius = 0.0, bool:setangle = true, Float:min_distance = 0.0, stopdelay = 250);
native FCNPC_GoToPlayer(npcid, playerid, type = FCNPC_MOVE_TYPE_AUTO, Float:speed = FCNPC_MOVE_SPEED_AUTO, mode = FCNPC_MOVE_MODE_AUTO, Float:radius = 0.0, bool:setangle = true, Float:min_distance = 0.0, Float:dist_check = 1.5, stopdelay = 250);
native FCNPC_Stop(npcid);
native FCNPC_IsMoving(npcid);
native FCNPC_IsMovingAtPlayer(npcid, playerid);
native FCNPC_GetDestinationPoint(npcid, &Float:x, &Float:y, &Float:z);

native FCNPC_AimAt(npcid, Float:x, Float:y, Float:z, bool:shoot = false, shoot_delay = -1, bool:setangle = true, Float:offset_from_x = 0.0, Float:offset_from_y = 0.0, Float:offset_from_z = 0.0);
native FCNPC_AimAtPlayer(npcid, playerid, bool:shoot = false, shoot_delay = -1, bool:setangle = true, Float:offset_x = 0.0, Float:offset_y = 0.0, Float:offset_z = 0.0, Float:offset_from_x = 0.0, Float:offset_from_y = 0.0, Float:offset_from_z = 0.0);
native FCNPC_StopAim(npcid);
native FCNPC_MeleeAttack(npcid, delay = -1, bool:fightstyle = false);
native FCNPC_StopAttack(npcid);
native FCNPC_IsAttacking(npcid);
native FCNPC_IsAiming(npcid);
native FCNPC_IsAimingAtPlayer(npcid, playerid);
native FCNPC_GetAimingPlayer(npcid);
native FCNPC_IsShooting(npcid);
native FCNPC_IsReloading(npcid);
native FCNPC_TriggerWeaponShot(npcid, weaponid, hittype, hitid, Float:x, Float:y, Float:z, bool:ishit = true, Float:offset_from_x = 0.0, Float:offset_from_y = 0.0, Float:offset_from_z = 0.0);

native FCNPC_EnterVehicle(npcid, vehicleid, seatid, type = FCNPC_MOVE_TYPE_WALK);
native FCNPC_ExitVehicle(npcid);

native FCNPC_PutInVehicle(npcid, vehicleid, seatid);
native FCNPC_RemoveFromVehicle(npcid);
native FCNPC_GetVehicleID(npcid);
native FCNPC_GetVehicleSeat(npcid);
native FCNPC_SetVehicleSiren(npcid, bool:status);
native FCNPC_IsVehicleSiren(npcid);
native FCNPC_SetVehicleHealth(npcid, Float:health);
native Float:FCNPC_GetVehicleHealth(npcid);
native FCNPC_SetVehicleHydraThrusters(npcid, direction);
native FCNPC_GetVehicleHydraThrusters(npcid);
native FCNPC_SetVehicleGearState(npcid, gear_state);
native FCNPC_GetVehicleGearState(npcid);

native FCNPC_SetSurfingOffsets(npcid, Float:x, Float:y, Float:z);
native FCNPC_GiveSurfingOffsets(npcid, Float:x, Float:y, Float:z);
native FCNPC_GetSurfingOffsets(npcid, &Float:x, &Float:y, &Float:z);
native FCNPC_SetSurfingVehicle(npcid, vehicleid);
native FCNPC_GetSurfingVehicle(npcid);
native FCNPC_SetSurfingObject(npcid, objectid);
native FCNPC_GetSurfingObject(npcid);
native FCNPC_StopSurfing(npcid);

native FCNPC_StartPlayingPlayback(npcid, file[] = "", recordid = FCNPC_INVALID_RECORD_ID, bool:auto_unload = false, Float:delta_x = 0.0, Float:delta_y  = 0.0, Float:delta_z  = 0.0, Float:delta_qw = 0.0, Float:delta_qx = 0.0, Float:delta_qy = 0.0, Float:delta_qz = 0.0);
native FCNPC_StopPlayingPlayback(npcid);
native FCNPC_PausePlayingPlayback(npcid);
native FCNPC_ResumePlayingPlayback(npcid);
native FCNPC_LoadPlayingPlayback(file[]);
native FCNPC_UnloadPlayingPlayback(recordid);
native FCNPC_SetPlayingPlaybackPath(npcid, path[]);
native FCNPC_GetPlayingPlaybackPath(npcid, path[], const size = sizeof(path));

native FCNPC_OpenNode(nodeid);
native FCNPC_CloseNode(nodeid);
native FCNPC_IsNodeOpen(nodeid);
native FCNPC_GetNodeType(nodeid);
native FCNPC_SetNodePoint(nodeid, pointid);
native FCNPC_GetNodePointPosition(nodeid, &Float:x, &Float:y, &Float:z);
native FCNPC_GetNodePointCount(nodeid);
native FCNPC_GetNodeInfo(nodeid, &vehnodes, &pednodes, &navinode);
native FCNPC_PlayNode(npcid, nodeid, move_type = FCNPC_MOVE_TYPE_AUTO, Float:speed = FCNPC_MOVE_SPEED_AUTO, mode = FCNPC_MOVE_MODE_AUTO, Float:radius = 0.0, bool:setangle = true);
native FCNPC_StopPlayingNode(npcid);
native FCNPC_PausePlayingNode(npcid);
native FCNPC_ResumePlayingNode(npcid);
native FCNPC_IsPlayingNode(npcid);
native FCNPC_IsPlayingNodePaused(npcid);

native FCNPC_CreateMovePath();
native FCNPC_DestroyMovePath(pathid);
native FCNPC_IsValidMovePath(pathid);
native FCNPC_AddPointToPath(pathid, Float:x, Float:y, Float:z);
native FCNPC_AddPointsToPath(pathid, Float:points[][3], const size = sizeof(points));
native FCNPC_AddPointsToPath2(pathid, Float:points_x[], Float:points_y[], Float:points_z[], const size = sizeof(points_x));
native FCNPC_RemovePointFromPath(pathid, pointid);
native FCNPC_IsValidMovePoint(pathid, pointid);
native FCNPC_GetMovePoint(pathid, pointid, &Float:x, &Float:y, &Float:z);
native FCNPC_GetNumberMovePoint(pathid);
native FCNPC_GoByMovePath(npcid, pathid, pointid = 0, type = FCNPC_MOVE_TYPE_AUTO, Float:speed = FCNPC_MOVE_SPEED_AUTO, mode = FCNPC_MOVE_MODE_AUTO, Float:radius = 0.0, bool:setangle = true, Float:min_distance = 0.0);

native FCNPC_SetMoveMode(npcid, mode);
native FCNPC_GetMoveMode(npcid);
native FCNPC_SetMinHeightPosCall(npcid, Float:height);
native Float:FCNPC_GetMinHeightPosCall(npcid);

native FCNPC_ShowInTabListForPlayer(npcid, forplayerid);
native FCNPC_HideInTabListForPlayer(npcid, forplayerid);
```
