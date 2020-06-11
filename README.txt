const Discord = require('discord.js');
const bot = new Discord.Client();
const token = ('NzE5NzMxNDU5NjE5MDI5MDU2.XuJVKg.NVxWF0V3Ahe6W_8g17KlaIxp2xA')
const prefix = 'do ';
const math = require('mathjs');
const r = "RANDOM";



bot.on('ready', () => {
    console.log(`${bot.user.tag} successfully logged in!`)
    bot.user.setActivity('the commands', ({type: "LISTENING"}))
})
 
bot.on('message', message => {
    let msg = message;
    let args = msg.content.slice(prefix.length).split(/ +/);
    let command = args.shift().toLowerCase();
    let cmd = command;
 


    if(message.content.startsWith(prefix + "ping")){
        message.channel.send("Pong\!")


    }
    if (command === 'help') {
        const embed = new Discord.MessageEmbed()
        .setTitle('Commands')
        .addField('General', `${prefix}help - Shows this message.\n${prefix}random - Shows a random number from randomNumber1 to randomNumber2\n${prefix}ping - pong\n ${prefix}kick - Kick's a random person.\n ${prefix}ban - Makes somebody go bye bye.`)
        .setColor(0x673AB7);
        msg.channel.send(embed);
    }
    if (command === 'random') {
        if(!args[0]) return msg.reply("You need to put a number in the first place.")
        if(!args[1]) return msg.reply("You need to choose the second nubmer.")
        msg.channel.send("Your random number is: " + Math.floor(Math.random() * args[1] + args[0]));
    }
    if (cmd === 'clear' || cmd === 'purge'){
        if(!msg.member.hasPermission("MANAGE_MESSAGES")) return msg.channel.send("You can't use this command!");
        if(!args[0]) return msg.channel.send("Atleast give me a number of messages you want me to delete.");
        msg.delete();
        msg.channel.bulkDelete(args[0]).catch(e => { msg.channel.send("You can only delete 100 messages at once.")});
        msg.channel.send(`Alright I deleted \`${args[0]} messages\``).then(m => m.delete({ timeout: 5000 }));
    }
    if(cmd === 'kick'){
        if(!msg.member.hasPermission('KICK_MEMBERS')) return msg.channel.send("You don't have permission to kick members.");
        let toKick = msg.mentions.members.first();
        let reason = args.slice(1).join(" ");
        if(!args[0]) return msg.channel.send('Who do you want me to kick though?');
        if(!toKick) return msg.channel.send(`From what I can see ${args[0]} isn't in the server.`);
        if(!reason) return msg.channel.send('Atleast give me a reason to kick the poor person.');
 
        if(!toKick.kickable){
            return msg.channel.send(':x: Hmm\, hmm for some reason I can\'t kik this person I wonder why. :x:');
        }
 
        if(toKick.kickable){
            let x = new Discord.MessageEmbed()
            .setTitle('Kick')
            .addField('Member Kicked', toKick)
            .addField('Kicked by', msg.author)
            .addField('Reason', reason)
            .setColor(r);
 
            msg.channel.send(x);
            toKick.kick();
        }
    }
    if(cmd === 'ban'){
        if(!msg.member.hasPermission("BAN_MEMBERS")) return msg.channel.send("You're not even allowed to ban ppl.'");
        let toBan = msg.mentions.members.first();
        let reason = args.slice(1).join(" ");
        if(!args[0]) return msg.channel.send('Who do you want me to ban?');
        if(!toBan) return msg.channel.send(`${args[0]} isn't in here dummie.`);
        if(!reason) return msg.channel.send('Atleast give me a reason to ban this poor person.');
 
        if(!toBan.bannable){
            return msg.channel.send(':x: Do you have perms to ban them? :x:');
        }
 
        if(toBan.bannable){
            let x = new Discord.MessageEmbed()
            .setTitle('Ban')
            .addField('Member Banned', toBan)
            .addField('Banned by', msg.author)
            .addField('Reason', reason)
            .setColor(r);
 
            msg.channel.send(x);
            toBan.ban();
        }
    }
})
 
bot.login(token);