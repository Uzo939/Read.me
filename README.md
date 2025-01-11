# Read.me

## Regular Expression and URL Matching

```javascript
var AF_URL_SCHEME = "(https:\\/\\/)(([^\\.]+)\\.)(.*\\/)(.*)";
var VALID_AF_URL_PARTS_LENGTH = 5;

// Example usage of the regular expression
var url = "https://subdomain.example.com/path/to/resource";
var match = url.match(new RegExp(AF_URL_SCHEME));

if (match) {
    console.log("Full match:", match[0]);
    console.log("Protocol:", match[1]);
    console.log("Subdomain:", match[2]);
    console.log("Domain:", match[3]);
    console.log("Path:", match[4]);
    console.log("Resource:", match[5]);
} else {
    console.log("No match found.");
}

# Use an official Node.js runtime as a parent image
FROM node:14

# Set the working directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Expose the port the app runs on
EXPOSE 8080

# Define the command to run the app
CMD ["npm", "start"]

version: '3.8'

services:
  app:
    build: .
    ports:
      - "8080:8080"
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    environment:
      NODE_ENV: development

// Create a new instance of MyEmitter
const myEmitter2 = new MyEmitter();

// Add an event listener for the 'event' event
myEmitter2.on('event', function(a, b) {
  console.log(a, b, this, this === myEmitter2);
});

// Emit the 'event' event with arguments 'a' and 'b'
myEmitter2.emit('event', 'a', 'b');

function getUserAgentData() {
    return new Promise(function(t) {
        isUACHSupported() ? navigator.userAgentData.getHighEntropyValues(["model", "platformVersion"]).then(function(e) {
            t({
                model: e.model,
                platformVersion: e.platformVersion
            });
        }).catch(function() {
            t();
        }) : t();
    });
}

QRCode(),
function() {
    var t = function t() {
        var e, i, o = arguments.length > 0 && void 0 !== arguments[0] ? arguments[0] : {
            afParameters: {}
        }, n = o.oneLinkURL, r = o.afParameters, a = (r = void 0 === r ? {} : r).mediaSource, s = r.campaign, l = r.channel, _ = r.ad, h = r.adSet, u = r.deepLinkValue, d = r.afSub1, g = r.afSub2, $ = r.afSub3, c = r.afSub4, f = r.afSub5, p = r.afCustom, m = r.googleClickIdKey, v = o.referrerSkipList, C = o.urlSkipList, A = null === (e = n || "") || void 0 === e ? void 0 : e.toString().match(AF_URL_SCHEME);
        if (!A || (null == A ? void 0 : A.length) < VALID_AF_URL_PARTS_LENGTH)
            return console.error("oneLinkURL is missing or not in the correct format, can't generate URL", n),
            null;
        if ((null == a ? void 0 : null === (i = a.keys) || void 0 === i ? void 0 : i.length) === 0 && !(null != a && a.defaultValue))
            return console.error("mediaSource is missing (default value was not supplied), can't generate URL", a),
            null;
        if (isSkippedURL({
            url: document.referrer,
            skipKeys: void 0 === v ? [] : v,
            errorMsg: "Generate url is skipped. HTTP referrer contains key:"
        }) || isSkippedURL({
            url: document.URL,
            skipKeys: void 0 === C ? [] : C,
            errorMsg: "Generate url is skipped. URL contains string:"
        }))
            return null;
        var T = {
            af_js_web: !0,
            af_ss_ver: window.AF_SMART_SCRIPT.version
        }
          , S = getURLParametersKV(window.location.search);
        if (a) {
            var b = getParameterValue(S, a);
            if (!b)
                return console.error("mediaSource was not found in the URL and default value was not supplied, can't generate URL", a),
                null;
            T.pid = b;
        }
        if (s && (T.c = getParameterValue(S, s)),
        l && (T.af_channel = getParameterValue(S, l)),
        _ && (T.af_ad = getParameterValue(S, _)),
        h && (T.af_adset = getParameterValue(S, h)),
        u && (T.deep_link_value = getParameterValue(S, u)),
        [d, g, $, c, f].forEach(function(t, e) {
            t && (T["af_sub".concat(e + 1)] = getParameterValue(S, t));
        }),
        m) {
            if (GCLID_EXCLUDE_PARAMS_KEYS.find(function(t) {
                return t === m;
            }))
                console.debug("Google Click Id ParamKey can't override AF Parameters keys", m);
            else {
                var O = getGoogleClickIdParameters(m, S);
                Object.keys(O).forEach(function(t) {
                    T[t] = O[t];
                });
            }
        }
        Array.isArray(p) && p.forEach(function(t) {
            if (null != t && t.paramKey) {
                var e = AF_CUSTOM_EXCLUDE_PARAMS_KEYS.find(function(e) {
                    return e === (null == t ? void 0 : t.paramKey);
                });
                (null == t ? void 0 : t.paramKey) === m || e ? console.debug("Custom parameter ParamKey can't override Google-Click-Id or AF Parameters keys", t) : T[[t.paramKey]] = getParameterValue(S, t);
            }
        });
        var y = stringifyParameters(T)
          , k = n + y.replace("&", "?");
        return console.debug("Generated OneLink URL", k),
        window.AF_SMART_SCRIPT.displayQrCode = function(t) {
            return k ? new QRCode(document.getElementById(t),{
                text: "".concat(k, "&af_ss_qr=true")
            }) : (console.debug("ClickURL is not valid"),
            null);
        }
        ,
        (k ? new Promise(function(t) {
            getUserAgentData().then(function(e) {
                var i = new URL(k);
                i.hostname = "impressions.onelink.me";
                if (e) {
                    i.pathname = "/ch".concat(i.pathname);
                    i.searchParams.append("af_ch_model", encodeURIComponent(e.model));
                    i.searchParams.append("af_ch_os_version", encodeURIComponent(e.platformVersion));
                }
                t(i.href);
            }).catch(function() {
                t();
            });
        }
        ) : (console.debug("ClickURL is not valid"),
        null)).then(function(t) {
            if (t) {
                window.AF_SMART_SCRIPT.fireImpressionsLink = function() {
                    var e = new Image(1, 1);
                    e.style.display = "none";
                    e.style.position = "absolute";
                    e.style.left = "-1px";
                    e.style.top = "-1px";
                    e.src = t;
                };
            }
        }),
        {
            clickURL: k
        };
    };
    window.AF_SMART_SCRIPT = {
        generateOneLinkURL: t,
        version: formatVersion
    };
}();


