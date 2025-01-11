# Read.me
var AF_URL_SCHEME = "(https:\\/\\/)(([^\\.]+)\\.)(.*\\/)(.*)";
var VALID_AF_URL_PARTS_LENGTH = 5;
var GOOGLE_CLICK_ID = "gclid";
var ASSOCIATED_AD_KEYWORD = "keyword";
var AF_KEYWORDS = "af_keywords";
var AF_CUSTOM_EXCLUDE_PARAMS_KEYS = ["pid", "c", "af_channel", "af_ad", "af_adset", "deep_link_value", "af_sub1", "af_sub2", "af_sub3", "af_sub4", "af_sub5"];
var GCLID_EXCLUDE_PARAMS_KEYS = ["pid", "c", "af_channel", "af_ad", "af_adset", "deep_link_value"];

function _typeof(t) {
    return (typeof Symbol === "function" && typeof Symbol.iterator === "symbol" ? function (t) {
        return typeof t;
    } : function (t) {
        return t && typeof Symbol === "function" && t.constructor === Symbol && t !== Symbol.prototype ? "symbol" : typeof t;
    })(t);
}

var stringifyParameters = function t() {
    var e = arguments.length > 0 && void 0 !== arguments[0] ? arguments[0] : {};
    var i = Object.keys(e).reduce(function (t, i) {
        return e[i] && (t += "&".concat(i, "=").concat(e[i])),
            t;
    }, "");
    console.debug("Generated OneLink parameters", i);
    return i;
};

var getParameterValue = function t(e) {
    var i = arguments.length > 1 && void 0 !== arguments[1] ? arguments[1] : {
        keys: [],
        overrideValues: {},
        defaultValue: ""
    };
    if (!(null != i && i.keys && Array.isArray(i.keys) || null != i && i.defaultValue)) {
        console.error("Parameter config structure is wrong", i);
        return null;
    }
    var o = i.keys;
    var n = i.overrideValues;
    var r = i.defaultValue;
    var a = void 0 === r ? "" : r;
    var s = (void 0 === o ? [] : o).find(function (t) {
        return !!e[t];
    });
    if (s) {
        var l = e[s];
        return (void 0 === n ? {} : n)[l] || l || a;
    }
    return a;
};

var getURLParametersKV = function t(e) {
    var i = e.replace("?", "").split("&").reduce(function (t, e) {
        var i = e.split("=");
        return i[0] && i[1] && (t[[i[0]]] = i[1]),
            t;
    }, {});
    return console.debug("Generated current parameters object", i),
        i;
};

var isIOS = function t(e) {
    return /iphone|ipad|ipod/i.test(e && e.toLowerCase());
};

var isUACHSupported = function t() {
    return (typeof navigator === "object" && "userAgentData" in navigator && "getHighEntropyValues" in navigator.userAgentData && !isIOS(navigator && navigator.userAgent));
};

var isSkippedURL = function t(e) {
    var i = e.url;
    var o = e.skipKeys;
    var n = e.errorMsg;
    if (i) {
        var r = i.toLowerCase();
        if (r) {
            var a = o.find(function (t) {
                return r.includes(t.toLowerCase());
            });
            return a && console.debug(n, a),
                !!a;
        }
    }
    return !1;
};

var getGoogleClickIdParameters = function t(e, i) {
    var o = i[GOOGLE_CLICK_ID];
    var n = {};
    if (o) {
        console.debug("This user comes from Google AdWords"),
            n[e] = o;
        var r = i[ASSOCIATED_AD_KEYWORD];
        r && (console.debug("There is a keyword associated with the ad"),
            n[AF_KEYWORDS] = r);
    } else
        console.debug("This user comes from SRN or custom network");
    return n;
};

/**
 * QRCode class for generating QR codes.
 * 
function QRCode() {
    var t, e, i = (typeof global === "object" && global && global.Object === Object && global, typeof self === "object" && self && self.Object === Object && self, global || self || Function("return this")()), o = (typeof exports === "object" && exports && !exports.nodeType && exports, exports && (typeof module === "object" && module && !module.nodeType && module, module && module.exports)), n = i.QRCode;
    function r(t, e, i) {
        this.mode = h.MODE_8BIT_BYTE,
        this.data = t,
        this.parsedData = [];
        for (var o = 0, n = this.data.length; o < n; o++) {
            var a = []
              , s = this.data.charCodeAt(o);
            e ? a[0] = s : s > 65536 ? (a[0] = 240 | (1835008 & s) >>> 18,
            a[1] = 128 | (258048 & s) >>> 12,
            a[2] = 128 | (4032 & s) >>> 6,
            a[3] = 128 | 63 & s) : s > 2048 ? (a[0] = 224 | (61440 & s) >>> 12,
            a[1] = 128 | (4032 & s) >>> 6,
            a[2] = 128 | 63 & s) : s > 128 ? (a[0] = 192 | (1984 & s) >>> 6,
            a[1] = 128 | 63 & s) : a[0] = s,
            this.parsedData.push(a);
        }
        this.parsedData = Array.prototype.concat.apply([], this.parsedData),
        i || this.parsedData.length == this.data.length || (this.parsedData.unshift(191),
        this.parsedData.unshift(187),
        this.parsedData.unshift(239));
    }
    function a(t, e) {
        this.typeNumber = t,
        this.errorCorrectLevel = e,
        this.modules = null,
        this.moduleCount = 0,
        this.dataCache = null,
        this.dataList = [];
    }
    r.prototype = {
        getLength: function t(e) {
            return this.parsedData.length;
        },
        write: function t(e) {
            for (var i = 0, o = this.parsedData.length; i < o; i++)
                e.put(this.parsedData[i], 8);
        }
    },
    a.prototype = {
        addData: function t(e, i, o) {
            var n = new r(e,i,o);
            this.dataList.push(n),
            this.dataCache = null;
        },
        isDark: function t(e, i) {
            if (e < 0 || this.moduleCount <= e || i < 0 || this.moduleCount <= i)
                throw Error(e + "," + i);
            return this.modules[e][i][0];
        },
        getEye: function t(e, i) {
            if (e < 0 || this.moduleCount <= e || i < 0 || this.moduleCount <= i)
                throw Error(e + "," + i);
            var o = this.modules[e][i];
            if (!o[1])
                return null;
            var n = "P" + o[1] + "_" + o[2];
            return "A" == o[2] && (n = "A" + o[1]),
            {
                isDark: o[0],
                type: n
            };
        },
        getModuleCount: function t() {
            return this.moduleCount;
        },
        make: function t() {
            this.makeImpl(!1, this.getBestMaskPattern());
        },
        makeImpl: function t(e, i) {
            this.moduleCount = 4 * this.typeNumber + 17,
            this.modules = Array(this.moduleCount);
            for (var o = 0; o < this.moduleCount; o++) {
                this.modules[o] = Array(this.moduleCount);
                for (var n = 0; n < this.moduleCount; n++)
                    this.modules[o][n] = [];
            }
            this.setupPositionProbePattern(0, 0, "TL"),
            this.setupPositionProbePattern(this.moduleCount - 7, 0, "BL"),
            this.setupPositionProbePattern(0, this.moduleCount - 7, "TR"),
            this.setupPositionAdjustPattern("A"),
            this.setupTimingPattern(),
            this.setupTypeInfo(e, i),
            this.typeNumber >= 7 && this.setupTypeNumber(e),
            null == this.dataCache && (this.dataCache = a.createData(this.typeNumber, this.errorCorrectLevel, this.dataList)),
            this.mapData(this.dataCache, i);
        },
        setupPositionProbePattern: function t(e, i, o) {
            for (var n = -1; n <= 7; n++)
                if (!(e + n <= -1) && !(this.moduleCount <= e + n))
                    for (var r = -1; r <= 7; r++)
                        i + r <= -1 || this.moduleCount <= i + r || (0 <= n && n <= 6 && (0 == r || 6 == r) || 0 <= r && r <= 6 && (0 == n || 6 == n) || 2 <= n && n <= 4 && 2 <= r && r <= 4 ? (this.modules[e + n][i + r][0] = !0,
                        this.modules[e + n][i + r][2] = o,
                        -0 == n || -0 == r || 6 == n || 6 == r ? this.modules[e + n][i + r][1] = "O" : this.modules[e + n][i + r][1] = "I") : this.modules[e + n][i + r][0] = !1);
        },
        getBestMaskPattern: function t() {
            for (var e = 0, i = 0, o = 0; o < 8; o++) {
                this.makeImpl(!0, o);
                var n = g.getLostPoint(this);
                (0 == o || e > n) && (e = n,
                i = o);
            }
            return i;
        },
        createMovieClip: function t(e, i, o) {
            var n = e.createEmptyMovieClip(i, o);
            this.make();
            for (var r = 0; r < this.modules.length; r++)
                for (var a = 1 * r, s = 0; s < this.modules[r].length; s++) {
                    var l = 1 * s;
                    this.modules[r][s][0] && (n.beginFill(0, 100),
                    n.moveTo(l, a),
                    n.lineTo(l + 1, a),
                    n.lineTo(l + 1, a + 1),
                    n.lineTo(l, a + 1),
                    n.endFill());
                }
            return n;
        },
        setupTimingPattern: function t() {
            for (var e = 8; e < this.moduleCount - 8; e++)
                null == this.modules[e][6][0] && (this.modules[e][6][0] = e % 2 == 0);
            for (var i = 8; i < this.moduleCount - 8; i++)
                null == this.modules[6][i][0] && (this.modules[6][i][0] = i % 2 == 0);
        },
        setupPositionAdjustPattern: function t(e) {
            for (var i = g.getPatternPosition(this.typeNumber), o = 0; o < i.length; o++)
                for (var n = 0; n < i.length; n++) {
                    var r = i[o]
                      , a = i[n];
                    if (null == this.modules[r][a][0])
                        for (var s = -2; s <= 2; s++)
                            for (var l = -2; l <= 2; l++)
                                -2 == s || 2 == s || -2 == l || 2 == l || 0 == s && 0 == l ? (this.modules[r + s][a + l][0] = !0,
                                this.modules[r + s][a + l][2] = e,
                                -2 == s || -2 == l || 2 == s || 2 == l ? this.modules[r + s][a + l][1] = "O" : this.modules[r + s][a + l][1] = "I") : this.modules[r + s][a + l][0] = !1;
                }
        },
        setupTypeNumber: function t(e) {
            for (var i = g.getBCHTypeNumber(this.typeNumber), o = 0; o < 18; o++) {
                var n = !e && (i >> o & 1) == 1;
                this.modules[Math.floor(o / 3)][o % 3 + this.moduleCount - 8 - 3][0] = n;
            }
            for (var o = 0; o < 18; o++) {
                var n = !e && (i >> o & 1) == 1;
                this.modules[o % 3 + this.moduleCount - 8 - 3][Math.floor(o / 3)][0] = n;
            }
        },
        setupTypeInfo: function t(e, i) {
            for (var o = this.errorCorrectLevel << 3 | i, n = g.getBCHTypeInfo(o), r = 0; r < 15; r++) {
                var a = !e && (n >> r & 1) == 1;
                r < 6 ? this.modules[r][8][0] = a : r < 8 ? this.modules[r + 1][8][0] = a : this.modules[this.moduleCount - 15 + r][8][0] = a;
            }
            for (var r = 0; r < 15; r++) {
                var a = !e && (n >> r & 1) == 1;
                r < 8 ? this.modules[8][this.moduleCount - r - 1][0] = a : r < 9 ? this.modules[8][15 - r - 1 + 1][0] = a : this.modules[8][15 - r - 1][0] = a;
            }
            this.modules[this.moduleCount - 8][8][0] = !e;
        },
        mapData: function t(e, i) {
            for (var o = -1, n = this.moduleCount - 1, r = 7, a = 0, s = this.moduleCount - 1; s > 0; s -= 2)
                for (6 == s && s--; ; ) {
                    for (var l = 0; l < 2; l++)
                        if (null == this.modules[n][s - l][0]) {
                            var _ = !1;
                            a < e.length && (_ = (e[a] >>> r & 1) == 1),
                            g.getMask(i, n, s - l) && (_ = !_),
                            this.modules[n][s - l][0] = _,
                            -1 == --r && (a++,
                            r = 7);
                        }
                    if ((n += o) < 0 || this.moduleCount <= n) {
                        n -= o,
                        o = -o;
                        break;
                    }
                }
        }
    },
    a.PAD0 = 236,
    a.PAD1 = 17,
    a.createData = function(t, e, i) {
        for (var o = p.getRSBlocks(t, e), n = new m, r = 0; r < i.length; r++) {
            var a = i[r];
            n.put(a.mode, 4),
            n.put(a.getLength(), g.getLengthInBits(a.mode, t)),
            a.write(n);
        }
        for (var s = 0, r = 0; r < o.length; r++)
            s += o[r].dataCount;
        if (n.getLengthInBits() > 8 * s)
            throw Error("code length overflow. (" + n.getLengthInBits() + ">" + 8 * s + ")");
        for (n.getLengthInBits() + 4 <= 8 * s && n.put(0, 4); n.getLengthInBits() % 8 != 0; )
            n.putBit(!1);
        for (; !(n.getLengthInBits() >= 8 * s) && (n.put(a.PAD0, 8),
        !(n.getLengthInBits() >= 8 * s)); ) {
            n.put(a.PAD1, 8);
        }
        return a.createBytes(n, o);
    }
    ,
    a.createBytes = function(t, e) {
        for (var i = 0, o = 0, n = 0, r = Array(e.length), a = Array(e.length), s = 0; s < e.length; s++) {
            var l = e[s].dataCount
              , _ = e[s].totalCount - l;
            o = Math.max(o, l),
            n = Math.max(n, _),
            r[s] = Array(l);
            for (var h = 0; h < r[s].length; h++)
                r[s][h] = 255 & t.buffer[h + i];
            i += l;
            var u = g.getErrorCorrectPolynomial(_)
              , d = new f(r[s],u.getLength() - 1).mod(u);
            a[s] = Array(u.getLength() - 1);
            for (var h = 0; h < a[s].length; h++) {
                var $ = h + d.getLength() - a[s].length;
                a[s][h] = $ >= 0 ? d.get($) : 0;
            }
        }
        for (var c = 0, h = 0; h < e.length; h++)
            c += e[h].totalCount;
        for (var p = Array(c), m = 0, h = 0; h < o; h++)
            for (var s = 0; s < e.length; s++)
                h < r[s].length && (p[m++] = r[s][h]);
        for (var h = 0; h < n; h++)
            for (var s = 0; s < e.length; s++)
                h < a[s].length && (p[m++] = a[s][h]);
        return p;
    }
    ;
    for (var s = {
        MODE_NUMBER: 1,
        MODE_ALPHA_NUM: 2,
        MODE_8BIT_BYTE: 4,
        MODE_KANJI: 8
    }, l = {
        L: 1,
        M: 0,
        Q: 3,
        H: 2
    }, _ = {
        PATTERN000: 0,
        PATTERN001: 1,
        PATTERN010: 2,
        PATTERN011: 3,
        PATTERN100: 4,
        PATTERN101: 5,
        PATTERN110: 6,
        PATTERN111: 7
    }, g = {
        PATTERN_POSITION_TABLE: [[], [6, 18], [6, 22], [6, 26], [6, 30], [6, 34], [6, 22, 38], [6, 24, 42], [6, 26, 46], [6, 28, 50], [6, 30, 54], [6, 32, 58], [6, 34, 62], [6, 26, 46, 66], [6, 26, 48, 70], [6, 26, 50, 74], [6, 30, 54, 78], [6, 30, 56, 82], [6, 30, 58, 86], [6, 34, 62, 90], [6, 28, 50, 72, 94], [6, 26, 50, 74, 98], [6, 30, 54, 78, 102], [6, 28, 54, 80, 106], [6, 32, 58, 84, 110], [6, 30, 58, 86, 114], [6, 34, 62, 90, 118], [6, 26, 50, 74, 98, 122], [6, 30, 54, 78, 102, 126], [6, 26, 52, 78, 104, 130], [6, 30, 56, 82, 108, 134], [6, 34, 60, 86, 112, 138], [6, 30, 58, 86, 114, 142], [6, 34, 62, 90, 118, 146], [6, 30, 54, 78, 102, 126, 150], [6, 24, 50, 76, 102, 128, 154], [6, 28, 54, 80, 106, 132, 158], [6, 32, 58, 84, 110, 136, 162], [6, 26, 54, 82, 110, 138, 166], [6, 30, 58, 86, 114, 142, 170]],
        G15: 1335,
        G18: 7973,
        G15_MASK: 21522,
        getBCHTypeInfo: function t(e) {
            for (var i = e << 10; g.getBCHDigit(i) - g.getBCHDigit(g.G15) >= 0; )
                i ^= g.G15 << g.getBCHDigit(i) - g.getBCHDigit(g.G15);
            return (e << 10 | i) ^ g.G15_MASK;
        },
        getBCHTypeNumber: function t(e) {
            for (var i = e << 12; g.getBCHDigit(i) - g.getBCHDigit(g.G18) >= 0; )
                i ^= g.G18 << g.getBCHDigit(i) - g.getBCHDigit(g.G18);
            return e << 12 | i;
        },
        getBCHDigit: function t(e) {
            for (var i = 0; 0 != e; )
                i++,
                e >>>= 1;
            return i;
        },
        getPatternPosition: function t(e) {
            return g.PATTERN_POSITION_TABLE[e - 1];
        },
        getMask: function t(e, i, o) {
            switch (e) {
            case _.PATTERN000:
                return (i + o) % 2 == 0;
            case _.PATTERN001:
                return i % 2 == 0;
            case _.PATTERN010:
                return o % 3 == 0;
            case _.PATTERN011:
                return (i + o) % 3 == 0;
            case _.PATTERN100:
                return (Math.floor(i / 2) + Math.floor(o / 3)) % 2 == 0;
            case _.PATTERN101:
                return i * o % 2 + i * o % 3 == 0;
            case _.PATTERN110:
                return (i * o % 2 + i * o % 3) % 2 == 0;
            case _.PATTERN111:
                return (i * o % 3 + (i + o) % 2) % 2 == 0;
            default:
                throw Error("bad maskPattern:" + e);
            }
        },
        getErrorCorrectPolynomial: function t(e) {
            for (var i = new f([1],0), o = 0; o < e; o++)
                i = i.multiply(new f([1, $.gexp(o)],0));
            return i;
        },
        getLengthInBits: function t(e, i) {
            if (1 <= i && i < 10)
                switch (e) {
                case s.MODE_NUMBER:
                    return 10;
                case s.MODE_ALPHA_NUM:
                    return 9;
                case s.MODE_8BIT_BYTE:
                case s.MODE_KANJI:
                    return 8;
                default:
                    throw Error("mode:" + e);
                }
            else if (i < 27)
                switch (e) {
                case s.MODE_NUMBER:
                    return 12;
                case s.MODE_ALPHA_NUM:
                    return 11;
                case s.MODE_8BIT_BYTE:
                    return 16;
                case s.MODE_KANJI:
                    return 10;
                default:
                    throw Error("mode:" + e);
                }
            else if (i < 41)
                switch (e) {
                case s.MODE_NUMBER:
                    return 14;
                case s.MODE_ALPHA_NUM:
                    return 13;
                case s.MODE_8BIT_BYTE:
                    return 16;
                case s.MODE_KANJI:
                    return 12;
                default:
                    throw Error("mode:" + e);
                }
            else
                throw Error("type:" + i);
        },
        getLostPoint: function t(e) {
            for (var i = e.getModuleCount(), o = 0, n = 0; n < i; n++)
                for (var r = 0; r < i; r++) {
                    for (var a = 0, s = e.isDark(n, r), l = -1; l <= 1; l++)
                        if (!(n + l < 0) && !(i <= n + l))
                            for (var _ = -1; _ <= 1; _++)
                                !(r + _ < 0) && !(i <= r + _) && (0 != l || 0 != _) && s == e.isDark(n + l, r + _) && a++;
                    a > 5 && (o += 3 + a - 5);
                }
            for (var n = 0; n < i - 1; n++)
                for (var r = 0; r < i - 1; r++) {
                    var h = 0;
                    e.isDark(n, r) && h++,
                    e.isDark(n + 1, r) && h++,
                    e.isDark(n, r + 1) && h++,
                    e.isDark(n + 1, r + 1) && h++,
                    (0 == h || 4 == h) && (o += 3);
                }
            for (var n = 0; n < i; n++)
                for (var r = 0; r < i - 6; r++)
                    e.isDark(n, r) && !e.isDark(n, r + 1) && e.isDark(n, r + 2) && e.isDark(n, r + 3) && e.isDark(n, r + 4) && !e.isDark(n, r + 5) && e.isDark(n, r + 6) && (o += 40);
            for (var r = 0; r < i; r++)
                for (var n = 0; n < i - 6; n++)
                    e.isDark(n, r) && !e.isDark(n + 1, r) && e.isDark(n + 2, r) && e.isDark(n + 3, r) && e.isDark(n + 4, r) && !e.isDark(n + 5, r) && e.isDark(n + 6, r) && (o += 40);
            for (var u = 0, r = 0; r < i; r++)
                for (var n = 0; n < i; n++)
                    e.isDark(n, r) && u++;
            return o + 10 * (Math.abs(100 * u / i / i - 50) / 5);
        }
    }, $ = {
        glog: function t(e) {
            if (e < 1)
                throw Error("glog(" + e + ")");
            return $.LOG_TABLE[e];
        },
        gexp: function t(e) {
            for (; e < 0; )
                e += 255;
            for (; e >= 256; )
                e -= 255;
            return $.EXP_TABLE[e];
        },
        EXP_TABLE: Array(256),
        LOG_TABLE: Array(256)
    }, c = 0; c < 8; c++)
        $.EXP_TABLE[c] = 1 << c;
    for (var c = 8; c < 256; c++)
        $.EXP_TABLE[c] = $.EXP_TABLE[c - 4] ^ $.EXP_TABLE[c - 5] ^ $.EXP_TABLE[c - 6] ^ $.EXP_TABLE[c - 8];
    for (var c = 0; c < 255; c++)
        $.LOG_TABLE[$.EXP_TABLE[c]] = c;
    function f(e, i) {
        if (e.length == t)
            throw Error(e.length + "/" + i);
        for (var o = 0; o < e.length && 0 == e[o]; )
            o++;
        this.num = Array(e.length - o + i);
        for (var n = 0; n < e.length - o; n++)
            this.num[n] = e[n + o];
    }
    function p(t, e) {
        this.totalCount = t,
        this.dataCount = e;
    }
    function m() {
        this.buffer = [],
        this.length = 0;
    }
    f.prototype = {
        get: function t(e) {
            return this.num[e];
        },
        getLength: function t() {
            return this.num.length;
        },
        multiply: function t(e) {
            for (var i = Array(this.getLength() + e.getLength() - 1), o = 0; o < this.getLength(); o++)
                for (var n = 0; n < e.getLength(); n++)
                    i[o + n] ^= $.gexp($.glog(this.get(o)) + $.glog(e.get(n)));
            return new f(i,0);
        },
        mod: function t(e) {
            if (this.getLength() - e.getLength() < 0)
                return this;
            for (var i = $.glog(this.get(0)) - $.glog(e.get(0)), o = Array(this.getLength()), n = 0; n < this.getLength(); n++)
                o[n] = this.get(n);
            for (var n = 0; n < e.getLength(); n++)
                o[n] ^= $.gexp($.glog(e.get(n)) + i);
            return new f(o,0).mod(e);
        }
    },
    p.RS_BLOCK_TABLE = [[1, 26, 19], [1, 26, 16], [1, 26, 13], [1, 26, 9], [1, 44, 34], [1, 44, 28], [1, 44, 22], [1, 44, 16], [1, 70, 55], [1, 70, 44], [2, 35, 17], [2, 35, 13], [1, 100, 80], [2, 50, 32], [2, 50, 24], [4, 25, 9], [1, 134, 108], [2, 67, 43], [2, 33, 15, 2, 34, 16], [2, 33, 11, 2, 34, 12], [2, 86, 68], [4, 43, 27], [4, 43, 19], [4, 43, 15], [2, 98, 78], [4, 49, 31], [2, 32, 14, 4, 33, 15], [4, 39, 13, 1, 40, 14], [2, 121, 97], [2, 60, 38, 2, 61, 39], [4, 40, 18, 2, 41, 19], [4, 40, 14, 2, 41, 15], [2, 146, 116], [3, 58, 36, 2, 59, 37], [4, 36, 16, 4, 37, 17], [4, 36, 12, 4, 37, 13], [2, 86, 68, 2, 87, 69], [4, 69, 43, 1, 70, 44], [6, 43, 19, 2, 44, 20], [6, 43, 15, 2, 44, 16], [4, 101, 81], [1, 80, 50, 4, 81, 51], [4, 50, 22, 4, 51, 23], [3, 36, 12, 8, 37, 13], [2, 116, 92, 2, 117, 93], [6, 58, 36, 2, 59, 37], [4, 46, 20, 6, 47, 21], [7, 42, 14, 4, 43, 15], [4, 133, 107], [8, 59, 37, 1, 60, 38], [8, 44, 20, 4, 45, 21], [12, 33, 11, 4, 34, 12], [3, 145, 115, 1, 146, 116], [4, 64, 40, 5, 65, 41], [11, 36, 16, 5, 37, 17], [11, 36, 12, 5, 37, 13], [5, 109, 87, 1, 110, 88], [5, 65, 41, 5, 66, 42], [5, 54, 24, 7, 55, 25], [11, 36, 12, 7, 37, 13], [5, 122, 98, 1, 123, 99], [7, 73, 45, 3, 74, 46], [15, 43, 19, 2, 44, 20], [3, 45, 15, 13, 46, 16], [1, 135, 107, 5, 136, 108], [10, 74, 46, 1, 75, 47], [1, 50, 22, 15, 51, 23], [2, 42, 14, 17, 43, 15], [5, 150, 120, 1, 151, 121], [9, 69, 43, 4, 70, 44], [17, 50, 22, 1, 51, 23], [2, 42, 14, 19, 43, 15], [3, 141, 113, 4, 142, 114], [3, 70, 44, 11, 71, 45], [17, 47, 21, 4, 48, 22], [9, 39, 13, 16, 40, 14], [3, 135, 107, 5, 136, 108], [3, 67, 41, 13, 68, 42], [15, 54, 24, 5, 55, 25], [15, 43, 15, 10, 44, 16], [4, 144, 116, 4, 145, 117], [17, 68, 42], [17, 50, 22, 6, 51, 23], [19, 46, 16, 6, 47, 17], [2, 139, 111, 7, 140, 112], [17, 74, 46], [7, 54, 24, 16, 55, 25], [34, 37, 13], [4, 151, 121, 5, 152, 122], [4, 75, 47, 14, 76, 48], [11, 54, 24, 14, 55, 25], [16, 45, 15, 14, 46, 16], [6, 147, 117, 4, 148, 118], [6, 73, 45, 14, 74, 46], [11, 54, 24, 16, 55, 25], [30, 46, 16, 2, 47, 17], [8, 132, 106, 4, 133, 107], [8, 75, 47, 13, 76, 48], [7, 54, 24, 22, 55, 25], [22, 45, 15, 13, 46, 16], [10, 142, 114, 2, 143, 115], [19, 74, 46, 4, 75, 47], [28, 50, 22, 6, 51, 23], [33, 46, 16, 4, 47, 17], [8, 152, 122, 4, 153, 123], [22, 73, 45, 3, 74, 46], [8, 53, 23, 26, 54, 24], [12, 45, 15, 28, 46, 16], [3, 147, 117, 10, 148, 118], [3, 73, 45, 23, 74, 46], [4, 54, 24, 31, 55, 25], [11, 45, 15, 31, 46, 16], [7, 146, 116, 7, 147, 117], [21, 73, 45, 7, 74, 46], [1, 53, 23, 37, 54, 24], [19, 45, 15, 26, 46, 16], [5, 145, 115, 10, 146, 116], [19, 75, 47, 10, 76, 48], [15, 54, 24, 25, 55, 25], [23, 45, 15, 25, 46, 16], [13, 145, 115, 3, 146, 116], [2, 74, 46, 29, 75, 47], [42, 54, 24, 1, 55, 25], [23, 45, 15, 28, 46, 16], [17, 145, 115], [10, 74, 46, 23, 75, 47], [10, 54, 24, 35, 55, 25], [19, 45, 15, 35, 46, 16], [17, 145, 115, 1, 146, 116], [14, 74, 46, 21, 75, 47], [29, 54, 24, 19, 55, 25], [11, 45, 15, 46, 46, 16], [13, 145, 115, 6, 146, 116], [14, 74, 46, 23, 75, 47], [44, 54, 24, 7, 55, 25], [59, 46, 16, 1, 47, 17], [12, 151, 121, 7, 152, 122], [12, 75, 47, 26, 76, 48], [39, 54, 24, 14, 55, 25], [22, 45, 15, 41, 46, 16], [6, 151, 121, 14, 152, 122], [6, 75, 47, 34, 76, 48], [46, 54, 24, 10, 55, 25], [2, 45, 15, 64, 46, 16], [17, 152, 122, 4, 153, 123], [29, 74, 46, 14, 75, 47], [49, 54, 24, 10, 55, 25], [24, 45, 15, 46, 46, 16], [4, 152, 122, 18, 153, 123], [13, 74, 46, 32, 75, 47], [48, 54, 24, 14, 55, 25], [42, 45, 15, 32, 46, 16], [20, 147, 117, 4, 148, 118], [40, 75, 47, 7, 76, 48], [43, 54, 24, 22, 55, 25], [10, 45, 15, 67, 46, 16], [19, 148, 118, 6, 149, 119], [18, 75, 47, 31, 76, 48], [34, 54, 24, 34, 55, 25], [20, 45, 15, 61, 46, 16]],
    p.getRSBlocks = function(e, i) {
        var o = p.getRsBlockTable(e, i);
        if (o == t)
            throw Error("bad rs block @ typeNumber:" + e + "/errorCorrectLevel:" + i);
        for (var n = o.length / 3, r = [], a = 0; a < n; a++)
            for (var s = o[3 * a + 0], l = o[3 * a + 1], _ = o[3 * a + 2], h = 0; h < s; h++)
                r.push(new p(l,_));
        return r;
    }
    ,
    p.getRsBlockTable = function(e, i) {
        switch (i) {
        case l.L:
            return p.RS_BLOCK_TABLE[(e - 1) * 4 + 0];
        case l.M:
            return p.RS_BLOCK_TABLE[(e - 1) * 4 + 1];
        case l.Q:
            return p.RS_BLOCK_TABLE[(e - 1) * 4 + 2];
        case l.H:
            return p.RS_BLOCK_TABLE[(e - 1) * 4 + 3];
        default:
            return t;
        }
    }
    ,
    m.prototype = {
        get: function t(e) {
            var i = Math.floor(e / 8);
            return (this.buffer[i] >>> 7 - e % 8 & 1) == 1;
        },
        put: function t(e, i) {
            for (var o = 0; o < i; o++)
                this.putBit((e >>> i - o - 1 & 1) == 1);
        },
        getLengthInBits: function t() {
            return this.length;
        },
        putBit: function t(e) {
            var i = Math.floor(this.length / 8);
            this.buffer.length <= i && this.buffer.push(0),
            e && (this.buffer[i] |= 128 >>> this.length % 8),
            this.length++;
        }
    };
    var v = [[17, 14, 11, 7], [32, 26, 20, 14], [53, 42, 32, 24], [78, 62, 46, 34], [106, 84, 60, 44], [134, 106, 74, 58], [154, 122, 86, 64], [192, 152, 108, 84], [230, 180, 130, 98], [271, 213, 151, 119], [321, 251, 177, 137], [367, 287, 203, 155], [425, 331, 241, 177], [458, 362, 258, 194], [520, 412, 292, 220], [586, 450, 322, 250], [644, 504, 364, 280], [718, 560, 394, 310], [792, 624, 442, 338], [858, 666, 482, 382], [929, 711, 509, 403], [1003, 779, 565, 439], [1091, 857, 611, 461], [1171, 911, 661, 511], [1273, 997, 715, 535], [1367, 1059, 751, 593], [1465, 1125, 805, 625], [1528, 1190, 868, 658], [1628, 1264, 908, 698], [1732, 1370, 982, 742], [1840, 1452, 1030, 790], [1952, 1538, 1112, 842], [2068, 1628, 1168, 898], [2188, 1722, 1228, 958], [2303, 1809, 1283, 983], [2431, 1911, 1351, 1051], [2563, 1989, 1423, 1093], [2699, 2099, 1499, 1139], [2809, 2213, 1579, 1219], [2953, 2331, 1663, 1273]];
    function C() {
        var t = !1
          , e = navigator.userAgent;
        if (/android/i.test(e)) {
            t = !0;
            var i = e.toString().match(/android ([0-9]\.[0-9])/i);
            i && i[1] && (t = parseFloat(i[1]));
        }
        return t;
    }
    var A, T = "undefined" != typeof CanvasRenderingContext2D ? function() {
        function t() {
            if ("svg" == this._htOption.drawer) {
                var t = this._oContext.getSerializedSvg(!0);
                this.dataURL = t,
                this._el.innerHTML = t;
            } else
                try {
                    var e = this._elCanvas.toDataURL("image/png");
                    this.dataURL = e;
                } catch (i) {
                    console.error(i);
                }
            this._htOption.onRenderingEnd && (this.dataURL || console.error("Can not get base64 data, please check: 1. Published the page and image to the server 2. The image request support CORS 3. Configured `crossOrigin:'anonymous'` option"),
            this._htOption.onRenderingEnd(this._htOption, this.dataURL));
        }
        if (n._android && n._android <= 2.1) {
            var e = 1 / window.devicePixelRatio
              , i = CanvasRenderingContext2D.prototype.drawImage;
            CanvasRenderingContext2D.prototype.drawImage = function(t, o, n, r, a, s, l, _, h) {
                if ("nodeName"in t && /img/i.test(t.nodeName))
                    for (var u = arguments.length - 1; u >= 1; u--)
                        arguments[u] = arguments[u] * e;
                else
                    void 0 === _ && (arguments[1] *= e,
                    arguments[2] *= e,
                    arguments[3] *= e,
                    arguments[4] *= e);
                i.apply(this, arguments);
            }
            ;
        }
        function o(t, e) {
            var i = this;
            if (i._fFail = e,
            i._fSuccess = t,
            null === i._bSupportDataURI) {
                var o = document.createElement("img")
                  , n = function t() {
                    i._bSupportDataURI = !1,
                    i._fFail && i._fFail.call(i);
                }
                  , r = function t() {
                    i._bSupportDataURI = !0,
                    i._fSuccess && i._fSuccess.call(i);
                };
                o.onabort = n,
                o.onerror = n,
                o.onload = r,
                o.src = "data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==";
                return;
            }
            !0 === i._bSupportDataURI && i._fSuccess ? i._fSuccess.call(i) : !1 === i._bSupportDataURI && i._fFail && i._fFail.call(i);
        }
        var r = function t(e, i) {
            this._bIsPainted = !1,
            this._android = C(),
            this._el = e,
            this._htOption = i,
            "svg" == this._htOption.drawer ? (this._oContext = {},
            this._elCanvas = {}) : (this._elCanvas = document.createElement("canvas"),
            this._el.appendChild(this._elCanvas),
            this._oContext = this._elCanvas.getContext("2d")),
            this._bSupportDataURI = null,
            this.dataURL = null;
        };
        return r.prototype.draw = function(t) {
            var e = this._htOption;
            e.title || e.subTitle || (e.height -= e.titleHeight,
            e.titleHeight = 0);
            var i = t.getModuleCount()
              , o = Math.round(e.width / i)
              , n = Math.round((e.height - e.titleHeight) / i);
            o <= 1 && (o = 1),
            n <= 1 && (n = 1),
            e.width = o * i,
            e.height = n * i + e.titleHeight,
            e.quietZone = Math.round(e.quietZone),
            this._elCanvas.width = e.width + 2 * e.quietZone,
            this._elCanvas.height = e.height + 2 * e.quietZone,
            "canvas" != this._htOption.drawer && (this._oContext = new C2S(this._elCanvas.width,this._elCanvas.height)),
            this.clear();
            var r = this._oContext;
            r.lineWidth = 0,
            r.fillStyle = e.colorLight,
            r.fillRect(0, 0, this._elCanvas.width, this._elCanvas.height);
            var a = this;
            function s() {
                e.quietZone > 0 && e.quietZoneColor && (r.lineWidth = 0,
                r.fillStyle = e.quietZoneColor,
                r.fillRect(0, 0, a._elCanvas.width, e.quietZone),
                r.fillRect(0, e.quietZone, e.quietZone, a._elCanvas.height - 2 * e.quietZone),
                r.fillRect(a._elCanvas.width - e.quietZone, e.quietZone, e.quietZone, a._elCanvas.height - 2 * e.quietZone),
                r.fillRect(0, a._elCanvas.height - e.quietZone, a._elCanvas.width, e.quietZone));
            }
            if (e.backgroundImage) {
                var l = new Image;
                l.onload = function() {
                    r.globalAlpha = 1,
                    r.globalAlpha = e.backgroundImageAlpha;
                    var i = r.imageSmoothingQuality
                      , o = r.imageSmoothingEnabled;
                    r.imageSmoothingEnabled = !0,
                    r.imageSmoothingQuality = "high",
                    r.drawImage(l, 0, e.titleHeight, e.width + 2 * e.quietZone, e.height + 2 * e.quietZone - e.titleHeight),
                    r.imageSmoothingEnabled = o,
                    r.imageSmoothingQuality = i,
                    r.globalAlpha = 1,
                    _.call(a, t);
                }
                ,
                null != e.crossOrigin && (l.crossOrigin = e.crossOrigin),
                l.originalSrc = e.backgroundImage,
                l.src = e.backgroundImage;
            } else
                _.call(a, t);
            function _(t) {
                e.onRenderingStart && e.onRenderingStart(e);
                for (var a = 0; a < i; a++)
                    for (var l = 0; l < i; l++) {
                        var _, h, u = l * o + e.quietZone, d = a * n + e.quietZone, g = t.isDark(a, l), $ = t.getEye(a, l), c = e.dotScale;
                        r.lineWidth = 0,
                        $ ? (_ = e[$.type] || e[$.type.substring(0, 2)] || e.colorDark,
                        h = e.colorLight) : e.backgroundImage ? (h = "rgba(0,0,0,0)",
                        6 == a ? e.autoColor ? (_ = e.timing_H || e.timing || e.autoColorDark,
                        h = e.autoColorLight) : _ = e.timing_H || e.timing || e.colorDark : 6 == l ? e.autoColor ? (_ = e.timing_V || e.timing || e.autoColorDark,
                        h = e.autoColorLight) : _ = e.timing_V || e.timing || e.colorDark : e.autoColor ? (_ = e.autoColorDark,
                        h = e.autoColorLight) : _ = e.colorDark) : (_ = 6 == a ? e.timing_H || e.timing || e.colorDark : 6 == l && (e.timing_V || e.timing) || e.colorDark,
                        h = e.colorLight),
                        r.strokeStyle = g ? _ : h,
                        r.fillStyle = g ? _ : h,
                        $ ? (c = "AO" == $.type ? e.dotScaleAO : "AI" == $.type ? e.dotScaleAI : 1,
                        e.backgroundImage && e.autoColor ? (_ = ("AO" == $.type ? e.AI : e.AO) || e.autoColorDark,
                        h = e.autoColorLight) : _ = ("AO" == $.type ? e.AI : e.AO) || _,
                        g = $.isDark,
                        r.fillRect(u + o * (1 - c) / 2, e.titleHeight + d + n * (1 - c) / 2, o * c, n * c)) : 6 == a ? (c = e.dotScaleTiming_H,
                        r.fillRect(u + o * (1 - c) / 2, e.titleHeight + d + n * (1 - c) / 2, o * c, n * c)) : 6 == l ? (c = e.dotScaleTiming_V,
                        r.fillRect(u + o * (1 - c) / 2, e.titleHeight + d + n * (1 - c) / 2, o * c, n * c)) : (e.backgroundImage,
                        r.fillRect(u + o * (1 - c) / 2, e.titleHeight + d + n * (1 - c) / 2, o * c, n * c)),
                        1 == e.dotScale || $ || (r.strokeStyle = e.colorLight);
                    }
                if (e.title && (r.fillStyle = e.titleBackgroundColor,
                r.fillRect(0, 0, this._elCanvas.width, e.titleHeight + e.quietZone),
                r.font = e.titleFont,
                r.fillStyle = e.titleColor,
                r.textAlign = "center",
                r.fillText(e.title, this._elCanvas.width / 2, +e.quietZone + e.titleTop)),
                e.subTitle && (r.font = e.subTitleFont,
                r.fillStyle = e.subTitleColor,
                r.fillText(e.subTitle, this._elCanvas.width / 2, +e.quietZone + e.subTitleTop)),
                e.logo) {
                    var f = new Image
                      , p = this;
                    f.onload = function() {
                        var t, i, o, n, a, l, _, h, u, d, g, $;
                        t = f,
                        n = Math.round(e.width / 3.5),
                        n !== (a = Math.round(e.height / 3.5)) && (n = a),
                        e.logoMaxWidth ? n = Math.round(e.logoMaxWidth) : e.logoWidth && (n = Math.round(e.logoWidth)),
                        e.logoMaxHeight ? a = Math.round(e.logoMaxHeight) : e.logoHeight && (a = Math.round(e.logoHeight)),
                        void 0 === t.naturalWidth ? (i = t.width,
                        o = t.height) : (i = t.naturalWidth,
                        o = t.naturalHeight),
                        (e.logoMaxWidth || e.logoMaxHeight) && (e.logoMaxWidth && i <= n && (n = i),
                        e.logoMaxHeight && o <= a && (a = o),
                        i <= n && o <= a && (n = i,
                        a = o)),
                        l = (e.width + 2 * e.quietZone - n) / 2,
                        _ = (e.height + e.titleHeight + 2 * e.quietZone - a) / 2,
                        h = Math.min(n / i, a / o),
                        u = i * h,
                        d = o * h,
                        (e.logoMaxWidth || e.logoMaxHeight) && (n = u,
                        a = d,
                        l = (e.width + 2 * e.quietZone - n) / 2,
                        _ = (e.height + e.titleHeight + 2 * e.quietZone - a) / 2),
                        e.logoBackgroundTransparent || (r.fillStyle = e.logoBackgroundColor,
                        r.fillRect(l, _, n, a)),
                        g = r.imageSmoothingQuality,
                        $ = r.imageSmoothingEnabled,
                        r.imageSmoothingEnabled = !0,
                        r.imageSmoothingQuality = "high",
                        r.drawImage(t, l + (n - u) / 2, _ + (a - d) / 2, u, d),
                        r.imageSmoothingEnabled = $,
                        r.imageSmoothingQuality = g,
                        s(),
                        p._bIsPainted = !0,
                        p.makeImage();
                    }
                    ,
                    f.onerror = function(t) {
                        console.error(t);
                    }
                    ,
                    null != e.crossOrigin && (f.crossOrigin = e.crossOrigin),
                    f.originalSrc = e.logo,
                    f.src = e.logo;
                } else
                    s(),
                    this._bIsPainted = !0,
                    this.makeImage();
            }
        }
        ,
        r.prototype.makeImage = function() {
            this._bIsPainted && o.call(this, t);
        }
        ,
        r.prototype.isPainted = function() {
            return this._bIsPainted;
        }
        ,
        r.prototype.clear = function() {
            this._oContext.clearRect(0, 0, this._elCanvas.width, this._elCanvas.height),
            this._bIsPainted = !1;
        }
        ,
        r.prototype.remove = function() {
            this._oContext.clearRect(0, 0, this._elCanvas.width, this._elCanvas.height),
            this._bIsPainted = !1,
            this._el.innerHTML = "";
        }
        ,
        r.prototype.round = function(t) {
            return t ? Math.floor(1e3 * t) / 1e3 : t;
        }
        ,
        r;
    }() : ((A = function t(e, i) {
        this._el = e,
        this._htOption = i;
    }
    ).prototype.draw = function(t) {
        var e = this._htOption
          , i = this._el
          , o = t.getModuleCount()
          , n = Math.round(e.width / o)
          , r = Math.round((e.height - e.titleHeight) / o);
        n <= 1 && (n = 1),
        r <= 1 && (r = 1),
        this._htOption.width = n * o,
        this._htOption.height = r * o + e.titleHeight,
        this._htOption.quietZone = Math.round(this._htOption.quietZone);
        var a = []
          , s = ""
          , l = Math.round(n * e.dotScale)
          , _ = Math.round(r * e.dotScale);
        l < 4 && (l = 4,
        _ = 4);
        var h = e.colorDark
          , u = e.colorLight;
        if (e.backgroundImage) {
            e.autoColor ? (e.colorDark = "rgba(0, 0, 0, .6);filter:progid:DXImageTransform.Microsoft.Gradient(GradientType=0, StartColorStr='#99000000', EndColorStr='#99000000');",
            e.colorLight = "rgba(255, 255, 255, .7);filter:progid:DXImageTransform.Microsoft.Gradient(GradientType=0, StartColorStr='#B2FFFFFF', EndColorStr='#B2FFFFFF');") : e.colorLight = "rgba(0,0,0,0)";
            var d = '<div style="display:inline-block; z-index:-10;position:absolute;"><img src="' + e.backgroundImage + '" widht="' + (e.width + 2 * e.quietZone) + '" height="' + (e.height + 2 * e.quietZone) + '" style="opacity:' + e.backgroundImageAlpha + ";filter:alpha(opacity=" + 100 * e.backgroundImageAlpha + '); "/></div>';
            a.push(d);
        }
        if (e.quietZone && (s = "display:inline-block; width:" + (e.width + 2 * e.quietZone) + "px; height:" + (e.width + 2 * e.quietZone) + "px;background:" + e.quietZoneColor + "; text-align:center;"),
        a.push('<div style="font-size:0;' + s + '">'),
        a.push('<table  style="font-size:0;border:0;border-collapse:collapse; margin-top:' + e.quietZone + 'px;" border="0" cellspacing="0" cellspadding="0" align="center" valign="middle">'),
        a.push('<tr height="' + e.titleHeight + '" align="center"><td style="border:0;border-collapse:collapse;margin:0;padding:0" colspan="' + o + '">'),
        e.title) {
            var g = e.titleColor
              , $ = e.titleFont;
            a.push('<div style="width:100%;margin-top:' + e.titleTop + "px;color:" + g + ";font:" + $ + ";background:" + e.titleBackgroundColor + '">' + e.title + "</div>");
        }
        e.subTitle && a.push('<div style="width:100%;margin-top:' + (e.subTitleTop - e.titleTop) + "px;color:" + e.subTitleColor + "; font:" + e.subTitleFont + '">' + e.subTitle + "</div>"),
        a.push("</td></tr>");
        for (var c = 0; c < o; c++) {
            a.push('<tr style="border:0; padding:0; margin:0;" height="7">');
            for (var f = 0; f < o; f++) {
                var p = t.isDark(c, f)
                  , m = t.getEye(c, f);
                if (m) {
                    p = m.isDark;
                    var v = m.type
                      , C = e[v] || e[v.substring(0, 2)] || h;
                    a.push('<td style="border:0;border-collapse:collapse;padding:0;margin:0;width:' + n + "px;height:" + r + 'px;"><span style="width:' + n + "px;height:" + r + "px;background-color:" + (p ? C : u) + ';display:inline-block"></span></td>');
                } else {
                    var A = e.colorDark;
                    6 == c ? (A = e.timing_H || e.timing || h,
                    a.push('<td style="border:0;border-collapse:collapse;padding:0;margin:0;width:' + n + "px;height:" + r + "px;background-color:" + (p ? A : u) + ';"></td>')) : 6 == f ? (A = e.timing_V || e.timing || h,
                    a.push('<td style="border:0;border-collapse:collapse;padding:0;margin:0;width:' + n + "px;height:" + r + "px;background-color:" + (p ? A : u) + ';"></td>')) : a.push('<td style="border:0;border-collapse:collapse;padding:0;margin:0;width:' + n + "px;height:" + r + 'px;"><div style="display:inline-block;width:' + l + "px;height:" + _ + "px;background-color:" + (p ? A : e.colorLight) + ';"></div></td>');
                }
            }
            a.push("</tr>");
        }
        if (a.push("</table>"),
        a.push("</div>"),
        e.logo) {
            var T = new Image;
            null != e.crossOrigin && (T.crossOrigin = e.crossOrigin),
            T.src = e.logo;
            var S = e.width / 3.5
              , b = e.height / 3.5;
            S != b && (S = b),
            e.logoWidth && (S = e.logoWidth),
            e.logoHeight && (b = e.logoHeight);
            var O = "position:relative; z-index:1;display:table-cell;top:-" + ((e.height - e.titleHeight) / 2 + b / 2 + e.quietZone) + "px;text-align:center; width:" + S + "px; height:" + b + "px;line-height:" + S + "px; vertical-align: middle;";
            e.logoBackgroundTransparent || (O += "background:" + e.logoBackgroundColor),
            a.push('<div style="' + O + '"><img  src="' + e.logo + '"  style="max-width: ' + S + "px; max-height: " + b + 'px;" /> <div style=" display: none; width:1px;margin-left: -1px;"></div></div>');
        }
        e.onRenderingStart && e.onRenderingStart(e),
        i.innerHTML = a.join("");
        var y = i.childNodes[0]
          , k = (e.width - y.offsetWidth) / 2
          , L = (e.height - y.offsetHeight) / 2;
        k > 0 && L > 0 && (y.style.margin = L + "px " + k + "px"),
        this._htOption.onRenderingEnd && this._htOption.onRenderingEnd(this._htOption, null);
    }
    ,
    A.prototype.clear = function() {
        this._el.innerHTML = "";
    }
    ,
    A);
    (e = function e(i, o) {
        if (this._htOption = {
            width: 256,
            height: 256,
            typeNumber: 4,
            colorDark: "#000000",
            colorLight: "#ffffff",
            correctLevel: l.H,
            dotScale: 1,
            dotScaleTiming: 1,
            dotScaleTiming_H: t,
            dotScaleTiming_V: t,
            dotScaleA: 1,
            dotScaleAO: t,
            dotScaleAI: t,
            quietZone: 0,
            quietZoneColor: "rgba(0,0,0,0)",
            title: "",
            titleFont: "normal normal bold 16px Arial",
            titleColor: "#000000",
            titleBackgroundColor: "#ffffff",
            titleHeight: 0,
            titleTop: 30,
            subTitle: "",
            subTitleFont: "normal normal normal 14px Arial",
            subTitleColor: "#4F4F4F",
            subTitleTop: 60,
            logo: t,
            logoWidth: t,
            logoHeight: t,
            logoMaxWidth: t,
            logoMaxHeight: t,
            logoBackgroundColor: "#ffffff",
            logoBackgroundTransparent: !1,
            PO: t,
            PI: t,
            PO_TL: t,
            PI_TL: t,
            PO_TR: t,
            PI_TR: t,
            PO_BL: t,
            PI_BL: t,
            AO: t,
            AI: t,
            timing: t,
            timing_H: t,
            timing_V: t,
            backgroundImage: t,
            backgroundImageAlpha: 1,
            autoColor: !1,
            autoColorDark: "rgba(0, 0, 0, .6)",
            autoColorLight: "rgba(255, 255, 255, .7)",
            onRenderingStart: t,
            onRenderingEnd: t,
            version: 0,
            tooltip: !1,
            binary: !1,
            drawer: "canvas",
            crossOrigin: null,
            utf8WithoutBOM: !0
        },
        "string" == typeof o && (o = {
            text: o
        }),
        o)
            for (var n in o)
                this._htOption[n] = o[n];
        (this._htOption.version < 0 || this._htOption.version > 40) && (console.warn("QR Code version '" + this._htOption.version + "' is invalidate, reset to 0"),
        this._htOption.version = 0),
        (this._htOption.dotScale < 0 || this._htOption.dotScale > 1) && (console.warn(this._htOption.dotScale + " , is invalidate, dotScale must greater than 0, less than or equal to 1, now reset to 1. "),
        this._htOption.dotScale = 1),
        (this._htOption.dotScaleTiming < 0 || this._htOption.dotScaleTiming > 1) && (console.warn(this._htOption.dotScaleTiming + " , is invalidate, dotScaleTiming must greater than 0, less than or equal to 1, now reset to 1. "),
        this._htOption.dotScaleTiming = 1),
        this._htOption.dotScaleTiming_H ? (this._htOption.dotScaleTiming_H < 0 || this._htOption.dotScaleTiming_H > 1) && (console.warn(this._htOption.dotScaleTiming_H + " , is invalidate, dotScaleTiming_H must greater than 0, less than or equal to 1, now reset to 1. "),
        this._htOption.dotScaleTiming_H = 1) : this._htOption.dotScaleTiming_H = this._htOption.dotScaleTiming,
        this._htOption.dotScaleTiming_V ? (this._htOption.dotScaleTiming_V < 0 || this._htOption.dotScaleTiming_V > 1) && (console.warn(this._htOption.dotScaleTiming_V + " , is invalidate, dotScaleTiming_V must greater than 0, less than or equal to 1, now reset to 1. "),
        this._htOption.dotScaleTiming_V = 1) : this._htOption.dotScaleTiming_V = this._htOption.dotScaleTiming,
        (this._htOption.dotScaleA < 0 || this._htOption.dotScaleA > 1) && (console.warn(this._htOption.dotScaleA + " , is invalidate, dotScaleA must greater than 0, less than or equal to 1, now reset to 1. "),
        this._htOption.dotScaleA = 1),
        this._htOption.dotScaleAO ? (this._htOption.dotScaleAO < 0 || this._htOption.dotScaleAO > 1) && (console.warn(this._htOption.dotScaleAO + " , is invalidate, dotScaleAO must greater than 0, less than or equal to 1, now reset to 1. "),
        this._htOption.dotScaleAO = 1) : this._htOption.dotScaleAO = this._htOption.dotScaleA,
        this._htOption.dotScaleAI ? (this._htOption.dotScaleAI < 0 || this._htOption.dotScaleAI > 1) && (console.warn(this._htOption.dotScaleAI + " , is invalidate, dotScaleAI must greater than 0, less than or equal to 1, now reset to 1. "),
        this._htOption.dotScaleAI = 1) : this._htOption.dotScaleAI = this._htOption.dotScaleA,
        (this._htOption.backgroundImageAlpha < 0 || this._htOption.backgroundImageAlpha > 1) && (console.warn(this._htOption.backgroundImageAlpha + " , is invalidate, backgroundImageAlpha must between 0 and 1, now reset to 1. "),
        this._htOption.backgroundImageAlpha = 1),
        this._htOption.height = this._htOption.height + this._htOption.titleHeight,
        "string" == typeof i && (i = document.getElementById(i)),
        this._htOption.drawer && ("svg" == this._htOption.drawer || "canvas" == this._htOption.drawer) || (this._htOption.drawer = "canvas"),
        this._android = C(),
        this._el = i,
        this._oQRCode = null;
        var r = {};
        for (var n in this._htOption)
            r[n] = this._htOption[n];
        this._oDrawing = new T(this._el,r),
        this._htOption.text && this.makeCode(this._htOption.text);
    }
    ).prototype.makeCode = function(t) {
        this._oQRCode = new a(function t(e, i) {
            for (var o, n, r = i.correctLevel, s = 1, l = (o = e,
            n = encodeURI(o).toString().replace(/\%[0-9a-fA-F]{2}/g, "a"),
            n.length + (n.length != o.length ? 3 : 0)), _ = 0, h = v.length; _ < h; _++) {
                var u = 0;
                switch (r) {
                case l.L:
                    u = v[_][0];
                    break;
                case l.M:
                    u = v[_][1];
                    break;
                case l.Q:
                    u = v[_][2];
                    break;
                case l.H:
                    u = v[_][3];
                }
                if (l <= u)
                    break;
                s++;
            }
            if (s > v.length)
                throw Error("Too long data. the CorrectLevel." + ["M", "L", "H", "Q"][r] + " limit length is " + u);
            return 0 != i.version && (s <= i.version ? (s = i.version,
            i.runVersion = s) : (console.warn("QR Code version " + i.version + " too small, run version use " + s),
            i.runVersion = s)),
            s;
        }(t, this._htOption),this._htOption.correctLevel),
        this._oQRCode.addData(t, this._htOption.binary, this._htOption.utf8WithoutBOM),
        this._oQRCode.make(),
        this._htOption.tooltip && (this._el.title = t),
        this._oDrawing.draw(this._oQRCode);
    }
    ,
    e.prototype.makeImage = function() {
        "function" == typeof this._oDrawing.makeImage && (!this._android || this._android >= 3) && this._oDrawing.makeImage();
    }
    ,
    e.prototype.clear = function() {
        this._oDrawing.remove();
    }
    ,
    e.prototype.resize = function(t, e) {
        this._oDrawing._htOption.width = t,
        this._oDrawing._htOption.height = e,
        this._oDrawing.draw(this._oQRCode);
    }
    ,
    e.prototype.noConflict = function() {
        return n.QRCode === this && (n.QRCode = r),
        e;
    }
    ,
    e.CorrectLevel = l,
    "function" == typeof define && (define.amd || define.cmd) ? define([], function() {
        return e;
    }) : o ? ((o.exports = e).QRCode = e,
    exports.QRCode = e) : i.QRCode = e;
}
var version = "2.3.0"
  , formatVersion = version.replace(/\./g, "_");
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
    }
    );
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
docker-compose updocker-compose up

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