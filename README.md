# The BaseLayer AI: make

Usage:

```
nix flake metadata github:BaseLayer-AI/baselayer-ai-make
```

RTFM [make.txt](make.txt)

[AI.md](AI.md)

```nix
{
  description = "The BaseLayer AI: make";

  inputs = { nixpkgs.url = "https://flakehub.com/f/NixOS/nixpkgs/*"; };

  outputs = { self, nixpkgs }:
    let
      aarch64-darwin = import nixpkgs { system = "aarch64-darwin"; };
      aarch64-linux = import nixpkgs { system = "aarch64-linux"; };
      x86_64-linux = import nixpkgs { system = "x86_64-linux"; };

      default = { pkgs } : pkgs.writeTextFile { name = "make"; executable = true; destination = "/bin/make";
        text = ''#!${pkgs.stdenv.shell}
${pkgs.gnumake}/bin/make --version
${pkgs.gnumake}/bin/make
        '';};
    in {
      packages.aarch64-darwin.default = default { pkgs = aarch64-darwin; };
      packages.aarch64-linux.default = default { pkgs = aarch64-linux; };
      packages.x86_64-linux.default = default { pkgs = x86_64-linux; };
    };
}
```
