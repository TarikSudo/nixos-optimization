# NixOS Optimization Guide

A guide for cleaning and optimizing NixOS. NixOS, known for its robust package management and configuration capabilities, can accumulate unnecessary data over time. The commands listed here help maintain system efficiency and free up disk space.

## Verifying and Checking Contents
- **Command**: `sudo nix-store --verify --check-contents`
- **Purpose**: This command verifies the integrity of the Nix store. It checks for any corruption or inconsistencies in the stored files.

## Optimizing the Nix Store
- **Command**: `sudo nix-store --optimise`
- **Purpose**: Optimizes the Nix store by hard linking identical files to reduce disk space usage.

## Deleting Old Generations
> :warning: **Warning**: Before using the following commands, it's important to understand their impact. Deleting old generations will permanently remove those system states. Be cautious, especially when removing recent generations, as you might need them for rollback in case of any issues with newer configurations.

### User Environment
- **Delete All Old Generations**:
  - **Command**: `nix-env --delete-generations old`
  - **Purpose**: Removes all old user environment generations, freeing up space.

- **Delete Specific Generation**:
  - **Command**: `nix-env --delete-generations <generation-number>`
  - **Purpose**: Deletes a specific generation of the user environment. Replace `<generation-number>` with the actual number.
 
- **Delete Last Few Generations**:
  - **Command**: `nix-env --delete-generations +<number-of-generations>`
  - **Purpose**: Deletes the last specified number of generations. Replace `<number-of-generations>` with the desired count.
 
### Root Environment
- **Commands and purposes are similar to the user environment**, but require `sudo` for execution:
  - `sudo nix-env --delete-generations old`
  - `sudo nix-env --delete-generations <generation-number>`
  - `sudo nix-env --delete-generations +<number-of-generations>`
 
### System Profile
- **Delete All Old System Profile Generations**:
  - **Command**: `sudo nix-env --delete-generations old --profile /nix/var/nix/profiles/system`
  - **Purpose**: Cleans up all old system profile generations.

- **Delete Specific System Profile Generation**:
  - **Command**: `sudo nix-env --delete-generations <generation-number> --profile /nix/var/nix/profiles/system`
  - **Purpose**: Deletes a specific system profile generation.
 
- **Delete Last Few System Profile Generations**:
  - **Command**: `sudo nix-env --delete-generations +<number-of-generations> --profile /nix/var/nix/profiles/system`
  - **Purpose**: Removes the last specified number of system profile generations.
 
## Garbage Collection
- **User-Level:**:
  - **Command**: `nix-collect-garbage -d`
  - **Purpose**: Removes unused Nix store paths at the user level. The `-d` flag deletes old generations of profiles.

- **System-Level**:
  - **Command**: `sudo nix-collect-garbage -d`
  - **Purpose**: Performs garbage collection for the entire system, removing unused packages and configurations.

## Updating and Rebuilding NixOS
- **Command**: `sudo nixos-rebuild switch --upgrade`
- **Purpose**: Updates the system to the latest channel versions and rebuilds the NixOS configuration, applying any changes.
