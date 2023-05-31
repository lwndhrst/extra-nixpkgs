# custom-nixpkgs

Simple flake with additional packages I use personally, that are not available
in official [Nixpkgs](https://github.com/NixOS/nixpkgs).



## Usage

### Example for dev shell with odin tools (with `nix develop`)
```nix 
{
  inputs = {
    nixpkgs = {
      url = "github:nixos/nixpkgs/nixos-unstable";
    };

    custom-nixpkgs = {
      url = "github:lwndhrst/custom-nixpkgs";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { self, nixpkgs, custom-nixpkgs }:
    let 
      system = "x86_64-linux";
      pkgs = import nixpkgs {
        inherit system;
        overlays = [ custom-nixpkgs.overlays.default ];
      };

    in {
      devShells.${system}.default = pkgs.mkShell {
        packages = with pkgs; [
          customPkgs.ols

          # This flake should not affect normal nixpkgs at all
          sl
        ];

        buildInputs = with pkgs; [
          customPkgs.odin
        ];
      };
    };
}
```



## With `nix shell`

For use with `nix shell`, add this flake to the registry:

```
nix registry add customPkgs github:lwndhrst/custom-nixpkgs
```

Packages from this flake can then be run like so:

```
nix shell customPkgs#<package>
```



## List of packages in this flake

- [nitch](https://github.com/lwndhrst/nitch) (fork of [nitch](https://github.com/ssleert/nitch) with nerdfont 3.0.0 icon fix and nixos support)
- [odin](https://github.com/odin-lang/Odin)
- [ols](https://github.com/DanielGavin/ols)
- [path-of-building](https://github.com/PathOfBuildingCommunity/PathOfBuilding)
- [sddm-rose-pine](https://github.com/lwndhrst/sddm-rose-pine) (SDDM theme based on [SDDM Sugar Dark](https://github.com/MarianArlt/sddm-sugar-dark) with [Rose Pine](https://rosepinetheme.com/) palette)
