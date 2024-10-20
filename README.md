# Neovim

> Customized Neovim, ready for development out of the box.

## Credits

- [Neovim Plus Ultra](https://github.com/jakehamilton/neovim): This project provided the foundational ideas upon which this project builds.

## Try Without Installing

You can try this configuration out without committing to installing it on your system by running
the following command.

```nix
nix run github:dimonzozo/neovim
```

## Install

### Nix Profile

You can install this package imperatively with the following command.

```nix
nix profile install github:dimonzozo/neovim
```

### Nix Configuration

You can install this package by adding it as an input to your Nix flake.

```nix
{
	description = "My system flake";

	inputs = {
		nixpkgs.url = "github:nixos/nixpkgs/nixos-22.05";

		# Snowfall is not required, but will make configuration easier for you.
		snowfall-lib = {
			url = "github:snowfallorg/lib";
			inputs.nixpkgs.follows = "nixpkgs";
		};

		neovim = {
			url = "github:dimonzozo/neovim";
			inputs.nixpkgs.follows = "nixpkgs";
		};
	};

	outputs = inputs:
		inputs.snowfall-lib.mkFlake {
			inherit inputs;
			src = ./.;

			overlays = with inputs; [
				# Use the overlay provided by this flake.
				neovim.overlays.default

				# There is also a named overlay, though the output is the same.
				neovim.overlays."package/neovim"
			];
		};
}
```

If you've added the overlay from this flake, then in your system configuration
you can add the `dimonzozo.neovim` package.

```nix
{ pkgs }:

{
	environment.systemPackages = with pkgs; [
		dimonzozo.neovim
	];
}
```
