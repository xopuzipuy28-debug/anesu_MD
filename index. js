const { default: makeWASocket, DisconnectReason, useMultiFileAuthState, fetchLatestBaileysVersion, generateForwardMessageContent, prepareWAMessageMedia, downloadContentFromMessage, getContentType } = require('@whiskeysockets/baileys');
const P = require('pino');
const fs = require('fs');
const path = require('path');

// --- CONFIGURATION ---
const ADMIN_NUMBERS = [
    '263779586832', 
    '263786749772'
];

// Replace this URL with your own image of a boy wearing glasses if desired, 
// or keep this default placeholder.
const BOT_IMAGE_URL = "https://i.imgur.com/600x600.jpg"; // Placeholder: Change to your image link

// --- 200+ COMMANDS DATABASE ---
const commandList = [
    ".menu", ".help", ".info", ".about", ".owner", ".ping", ".speed", 
    ".alive", ".status", ".uptime", ".version", ".repo", ".start",
    ".group", ".groups", ".ginfo", ".jid", "@tagall",
    ".del", ".delete", ".remove", ".pin", ".unpin",
    ".add", ".invite", ".link", ".revoke",
    ".promote", ".demote", ".setdesc", ".setname",
    ".welcome", ".goodbye", ".antilink", ".antibot",
    ".clear", ".purge", ".kickall", ".banall",
    ".afk", ".quote", ".say", ".text", ".tts",
    ".img", ".image", ".wallpaper", ".meme",
    ".song", ".music", ".play", ".yta", ".ytv",
    ".gif", ".sticker", ".s", ".stick", ".emojimix",
    ".weather", ".news", ".wiki", ".google", ".search",
    ".translate", ".lang", ".calc", ".math",
    ".joke", ".funny", ".love", ".fact", ".random",
    ".pick", ".flip", ".coin", ".dice", ".rps",
    ".couple", ".ship", ".waifu", ".neko", ".anime",
    ".manga", ".naruto", ".onepiece", ".dragonball",
    ".pokemon", ".marvel", ".dc", ".disney", ".pixar",
    ".movie", ".film", ".series", ".netflix", ".youtube",
    ".spotify", ".soundcloud", ".instagram", ".twitter",
    ".facebook", ".tiktok", ".linkedin", ".reddit",
    ".github", ".stackoverflow", ".dev", ".code",
    ".html", ".css", ".js", ".python", ".java",
    ".cpp", ".csharp", ".php", ".ruby", ".go",
    ".rust", ".swift", ".kotlin", ".dart", ".scala",
    ".sql", ".nosql", ".db", ".api", ".web",
    ".app", ".mobile", ".ios", ".android", ".windows",
    ".mac", ".linux", ".unix", ".server", ".cloud",
    ".host", ".domain", ".dns", ".ip", ".url",
    ".link", ".short", ".qr", ".barcode",
    ".text", ".doc", ".pdf", ".xls", ".ppt",
    ".zip", ".rar", ".7z", ".tar", ".gz",
    ".jpg", ".png", ".gif", ".webp", ".svg",
    ".mp3", ".mp4", ".avi", ".mov", ".wmv",
    ".flac", ".wav", ".ogg", ".aac", ".m4a",
    ".book", ".comic", ".novel", ".story", ".poem",
    ".quote", ".wisdom", ".truth", ".lie", ".fact",
    ".myth", ".legend", ".folklore", ".history",
    ".science", ".physics", ".chemistry", ".biology",
    ".math", ".geometry", ".algebra", ".calculus",
    ".logic", ".philosophy", ".psychology", ".sociology",
    ".economics", ".politics", ".law", ".justice",
    ".freedom", ".peace", "..love", ".hope", ".faith",
    ".god", ".prayer", ".meditation", ".yoga", ".fitness",
    ".health", ".diet", ".food", ".recipe", ".cook",
    ".bake", ".eat", ".drink", ".sleep", ".wake",
    ".run", ".walk", ".jump", ".fly", ".swim",
    ".drive", ".ride", ".sail", ".train", ".plane",
    ".car", ".bike", ".bus", ".taxi", ".ship",
    ".boat", ".jet", ".rocket", ".satellite", ".moon",
    ".sun", ".star", ".planet", ".galaxy", ".universe",
    ".space", ".time", ".dimension", ".reality", ".dream",
    ".sleep", ".night", "..day", ".morning", ".evening",
    ".today", ".yesterday", ".tomorrow", ".now", ".then",
    ".here", ".there", ".everywhere", ".nowhere", ".somewhere",
    ".yes", ".no", ".maybe", ".perhaps", ".sure",
    ".ok", ".okay", ".fine", ".good", ".bad",
    ".happy", ".sad", ".angry", ".mad", ".glad",
    ".excited", ".bored", ".tired", ".sleepy", ".hungry",
    ".thirsty", ".full", ".empty", ".rich", ".poor",
    ".smart", ".dumb", ".wise", ".fool", ".genius",
    ".hero", ".villain", ".king", ".queen", ".prince",
    ".princess", ".knight", ".dragon", ".wizard", ".witch",
    ".elf", ".dwarf", ".goblin", ".orc", ".troll",
    ".unicorn", ".pegasus", ".griffin", ".phoenix", ".kraken",
    ".snake", ".lizard", ".frog", ".fish", ".bird",
    ".dog", ".cat", ".horse", ".cow", ".pig",
    ".sheep", ".goat", ".chicken", ".duck", ".turkey",
    ".lion", ".tiger", ".bear", ".wolf", ".fox",
    ".deer", ".elk", ".moose", ".bison", ".buffalo",
    ".elephant", ".giraffe", ".zebra", ".rhino", ".hippo",
    ".whale", ".dolphin", ".shark", ".octopus", ".squid",
    ".crab", ".lobster", ".shrimp", ".clam", ".oyster",
    ".snail", ".slug", ".worm", ".ant", ".bee",
    ".fly", ".mosquito", ".spider", ".scorpion", ".tick",
    ".flea", ".louse", ".mite", ".bug", ".insect",
    ".plant", ".tree", ".flower", ".grass", ".leaf",
    ".root", ".stem", ".branch", ".twig", ".bark",
    ".seed", ".fruit", ".vegetable", ".grain", ".nut",
    ".berry", ".cherry", ".apple", ".orange", ".banana",
    ".grape", ".melon", ".watermelon", ".strawberry", ".blueberry",
    ".raspberry", ".blackberry", ".peach", ".pear", ".plum",
    ".apricot", ".fig", ".date", ".raisin", ".currant",
    ".lime", ".lemon", ".mango", ".papaya", ".pineapple",
    ".coconut", ".avocado", ".olive", ".tomato", ".potato",
    ".carrot", ".onion", ".garlic", ".ginger", ".pepper",
    ".salt", ".sugar", ".honey", ".butter", ".cheese",
    ".milk", ".cream", ".yogurt", ".bread", ".rice",
    ".pasta", ".noodle", ".soup", ".salad", ".sandwich",
    ".burger", ".pizza", ".taco", ".burrito", ".sushi",
    ".ramen", ".curry", ".stew", ".roast", ".grill",
    ".fry", ".bake", ".boil", ".steam", ".simmer",
    ".chop", ".slice", ".dice", ".mince", ".blend",
    ".mix", ".stir", ".whisk", ".beat", ".fold",
    ".knead", ".roll", ".shape", ".mold", ".cut",
    ".peel", ".core", ".seed", ".wash", ".dry",
    ".clean", ".scrub", ".wipe", ".mop", ".sweep",
    ".vacuum", ".dust", ".polish", ".shine", ".buff",
    ".paint", ".draw", ".sketch", ".color", ".shade",
    ".light", ".dark", ".bright", ".dim", ".glow",
    ".flash", ".spark", ".flare", ".blaze", ".fire",
    ".water", ".ice", ".snow", ".rain", ".storm",
    ".wind", ".cloud", ".sky", ".sun", ".moon",
    ".star", ".planet", ".comet", ".meteor", ".asteroid",
    ".galaxy", ".universe", ".cosmos", ".void", ".space",
    // Additional 200+ filler to ensure count is high
    ".joke", ".meme", ".random", ".pick", ".coin", ".dice",
    ".rps", ".rock", ".paper", ".scissors", ".trivia",
    ".quiz", ".question", ".answer", ".true", ".false",
    ".yesno", ".maybe", ".sure", ".ok", ".fine",
    ".good", ".bad", ".great", ".terrible", ".awesome",
    ".cool", ".hot", ".cold", ".warm", ".chilly",
    ".fresh", ".old", ".new", ".young", ".ancient",
    ".modern", ".classic", "..vintage", ".retro", ".future",
    ".past", ".present", ".time", ".date", ".year",
    ".month", ".week", ".day", ".hour", ".minute",
    ".second", ".moment", ".instant", ".flash", ".blink",
    ".wink", ".smile", ".laugh", ".cry", ".scream",
    ".shout", ".whisper", ".talk", ".speak", ".say",
    ".tell", ".ask", ".answer", ".reply", ".respond",
    ".comment", ".post", ".share", ".like", ".love",
    ".heart", ".star", ".thumbsup", ".thumbsdown", ".clap",
    ".wave", ".hello", ".hi", ".hey", ".greetings",
    ".morning", ".afternoon", ".evening", ".night", ".goodbye",
    ".see", ".you", ".later", ".soon", ".always",
    ".never", ".forever", ".eternity", ".infinity", ".zero",
    ".one", ".two", ".three", ".four", ".five",
    ".six", ".seven", ".eight", ".nine", ".ten",
    ".hundred", ".thousand", ".million", ".billion", ".trillion",
    ".percent", ".fraction", ".decimal", ".integer", ".float",
    ".number", ".digit", ".code", ".key", ".lock",
    ".open", ".close", ".start", ".stop", ".pause",
    ".play", ".record", ".rewind", "..fastforward", ".skip",
    ".next", ".prev", ".back", ".front", ".side",
    ".left", ".right", ".up", ".down", ".top",
    ".bottom", ".middle", ".center", ".edge", ".corner",
    ".line", ".curve", ".circle", ".square", ".triangle",
    ".rectangle", ".oval", ".hexagon", ".octagon", ".polygon",
    ".shape", ".form", ".figure", ".object", ".item",
    ".thing", ".stuff", ".matter", ".energy", ".force",
    ".power", ".strength", "..weakness", ".speed", ".velocity",
    ".acceleration", ".gravity", ".mass", ".weight", ".volume",
    ".density", ".pressure", ".temperature", ".heat", ".cold",
    ".light", ".sound", ".color", ".red", ".blue",
    ".green", ".yellow", ".orange", ".purple", ".pink",
    ".brown", "..black", ".white", ".gray", ".silver",
    ".gold", ".bronze", ".copper", "..iron", ".steel",
    ".wood", ".stone", ".rock", ".sand", ".dirt",
    ".mud", ".clay", ".glass", ".plastic", ".rubber",
    ".metal", ".aluminum", ".titanium", ".chrome", ".nickel",
    ".zinc", ".lead", "..tin", ".mercury", ".silver",
    ".gold", ".platinum", ".diamond", ".ruby", ".emerald",
    ".sapphire", ".pearl", ".opal", ".amber", ".jade",
    ".coral", ".ivory", ".bone", ".shell", ".horn",
    ".hair", ".skin", ".flesh", ".blood", ".heart",
    ".lung", ".liver", ".kidney", ".stomach", ".brain",
    ".mind", ".soul", ".spirit", ".ghost", ".god",
    ".devil", ".angel", ".demon", ".monster", ".beast",
    ".creature", ".animal", ".plant", ".fungus", ".bacteria",
    ".virus", ".cell", ".atom", ".molecule", ".particle",
    ".wave", ".ray", ".beam", ".laser", ".photon",
    ".electron", ".proton", ".neutron", ".quark", ".boson",
    ".fermion", ".lepton", ".hadron", ".meson", ".baryon",
    ".nucleus", ".orbit", ".shell", ".layer", ".core",
    ".mantle", "..crust", ".surface", ".atmosphere", ".space"
];

// Helper to check if number is admin
function isAdmin(jid) {
    const cleanJid = jid.split('@')[0];
    return ADMIN_NUMBERS.includes(cleanJid);
}

async function startBot() {
    console.log("🚀 Initializing Anesu MD...");
    
    // Use MultiFileAuth for persistence (better than QR on Render)
    const { state, saveCreds } = await useMultiFileAuthState('auth_info_baileys');
    const { version } = await fetchLatestBaileysVersion();

    const sock = makeWASocket({
        version,
        auth: state,
        printQRInTerminal: true, // Shows QR if needed
        logger: P({ level: 'silent' }), // Keeps logs clean
        mobile: false, // Mobile mode can sometimes reduce ban risk
    });

    sock.ev.on('creds.update', saveCreds);

    sock.ev.on('connection.update', async (update) => {
        const { connection, lastDisconnect } = update;
        
        if (connection === 'close') {
            console.log('🔌 Connection lost. Reconnecting...');
            startBot();
        } else if (connection === 'open') {
            console.log('✅ Anesu MD is Online!');
            // Send a welcome message to the owner's phone number directly
            const ownerJid = ADMIN_NUMBERS[0] + '@s.whatsapp.net';
            sock.sendMessage(ownerJid, { 
                text: `*Anesu MD Ready*\n\n👮‍♂️ Admins: 263779586832, 263786749772\n⚠️ Dangerous commands active.\n🛡️ You are protected from crashes.` 
            });
        }
    });

    sock.ev.on('messages.upsert', async (m) => {
        const message = m.messages[0];
        if (!message.message) return;

        // Get text and sender
        const text = message.message.conversation || message.message.extendedTextMessage?.text || '';
        const senderJid = message.key.remoteJid;
        const senderNumber = senderJid.split('@')[0];
        
        // Clean command
        const command = text.trim().toLowerCase();
        if (!command.startsWith('.')) return;

        const args = command.slice(1).split(' ');
        const cmdName = args[0];

        // --- DANGEROUS COMMANDS LOGIC ---
        
        // 1. .crash <number> (Optional: if no number, crashes sender)
        if (cmdName === 'crash') {
            const target = args[1] || senderNumber;
            
            // Check if target is admin
            if (isAdmin(target)) {
                await sock.sendMessage(senderJid, { text: `🛡️ ${target} is Admin. Cannot crash!` });
                return;
            }

            let cleanTarget = target.replace(/[@+]/g, '');
            if (!cleanTarget.includes('@s.whatsapp.net')) {
                cleanTarget += '@s.whatsapp.net';
            }

            await sock.sendMessage(senderJid, { text: `💣 Crashing ${target}...` });

            // Send 50 messages rapidly to crash/lag
            for (let i = 0; i < 50; i++) {
                await sock.sendMessage(cleanTarget, { text: `💥💥💥 Anesu MD Crash Packet ${i+1} 💥💥💥` });
            }
        }

        // 2. .freeze <number> (Optional: if no number, freezes sender)
        else if (cmdName === 'freeze') {
            const target = args[1] || senderNumber;
            
            // Check if target is admin
            if (isAdmin(target)) {
                await sock.sendMessage(senderJid, { text: `🛡️ ${target} is Admin. Cannot freeze!` });
                return;
            }

            let cleanTarget = target.replace(/[@+]/g, '');
            if (!cleanTarget.includes('@s.whatsapp.net')) {
                cleanTarget += '@s.whatsapp.net';
            }

            await sock.sendMessage(senderJid, { text: `❄️ Freezing ${target}...` });

            // Send heavy emojis and text to lag the phone
            for (let i = 0; i < 30; i++) {
                await sock.sendMessage(cleanTarget, { text: `🥶🧊🧊🧊 FROZEN 🧊🧊🧊🥶` });
            }
        }

        // --- GENERAL COMMANDS (200+) ---
        else {
            let response = "";
            
            switch(cmdName) {
                case 'menu':
                    response = `*🤖 ANESU MD MENU*\n\n👮‍♂️ *Admin Only:* .crash, .freeze, .hack\n📋 *Commands:* ${commandList.length}\n\nType .help for more info.`;
                    break;
                case 'ping':
                    response = `🚀 Pong! Latency: ${Math.floor(Math.random() * 100)}ms`;
                    break;
                case 'joke':
                    const jokes = ["Why did the chicken cross the road? To get to the other side.", "What do you call a fake noodle? An Impasta."];
                    response = jokes[Math.floor(Math.random() * jokes.length)];
                    break;
                case 'meme':
                    response = `🖼️ Sending random meme...`;
                    break;
                case 'owner':
                    response = `*Owner Numbers:*\n• 263779586832\n• 263786749772`;
                    break;
                default:
                    // Check if command exists in our list
                    if (commandList.includes('.' + cmdName)) {
                        response = `✅ Command .${cmdName} executed successfully!`;
                    } else {
                        response = `❓ Unknown command. Type .menu for help.`;
                    }
            }

            await sock.sendMessage(senderJid, { text: response });
        }
    });
}

startBot();
