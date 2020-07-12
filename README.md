# Blazor Bits WebAssembly on GitHub Pages

**Automatically deployed on GitHub Pages ([https://jeepnl.github.io/BlazorBits/](https://jeepnl.github.io/BlazorBits/)) using GitHub Actions**

![gh-pages](https://github.com/JeepNL/BlazorBits/workflows/gh-pages/badge.svg)

#### [Change Log](CHANGELOG.md)

##### To deploy your Blazor WebAssembly App automatically to GitHub Pages you'll need to:

1. Have a 'master' and a 'development' branche. 
    - The 'master' branche is mandatory. GitHub Pages needs a 'master' branche. You can use any name for the 'development' branche.
2. Make the 'development' branche the default branche.
3. GitHub Actions will deploy to the 'master' branche
4. Follow [Davide Guida](https://twitter.com/DavideGuida82)'s blog post "[How to deploy Blazor webassembly on GitHub Pages using GitHub Actions](https://www.davideguida.com/how-to-deploy-blazor-webassembly-on-github-pages-using-github-actions/)"
5. Don't forget to enable GitHub Pages on the Master Branche: `Settings` -> `Options` -> `GitHub Pages`

#### Recipes

##### 1. Blazor WebAssembly Browser Compatibility Check

(_wwwroot/[index.html](BlazorBits/wwwroot/index.html)_)

    <!-- Start Check Browser Compatibility -->
    <script src="_framework/blazor.webassembly.js" autostart="false"></script>
    <script>
        // See: https://medium.com/@waelkdouh/how-to-detect-unsupported-browsers-under-a-blazor-webassembly-application-bc11ab0ee015
        // Check if webassembly is supported
        const webassemblySupported = (function () {
            try {
                if (typeof WebAssembly === "object"
                    && typeof WebAssembly.instantiate === "function") {
                    const module = new WebAssembly.Module(
                        Uint8Array.of(0x0, 0x61, 0x73, 0x6d,
                            0x01, 0x00, 0x00, 0x00));
                    if (module instanceof WebAssembly.Module)
                        return new WebAssembly.Instance(module)
                            instanceof WebAssembly.Instance;
                }
            } catch (e) {
            }
            return false;
        })();
        // Modern browsers e.g. Microsoft Edge
        if (webassemblySupported) {
            Blazor.start({});
        }
        // Older browsers e.g. IE11
        else {
            window.location = window.location + "BrowserNotSupported.html"; // NOTE: You'll need to create this file
        }
    </script>
    <!-- End Check Browser Compatibility -->

##### 2. Blazor Loading Screen

(_wwwroot/[index.html](BlazorBits/wwwroot/index.html)_)

    <app>
        <!-- Start Loading Screen -->
        <div id="message" style="position:absolute; top:30vh; width:100%; text-align:center">
            <h2>Blazor Bits GitHub Pages</h2>
            <p>The app is loading</p>
            <div class="spinner-border text-secondary" style="width: 3rem; height: 3rem;" role="status">
                <span class="sr-only">Loading...</span>
            </div>
        </div>
        <!-- End Loading Screen -->
    </app>

