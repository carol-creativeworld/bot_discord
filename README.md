import discord
import os
import random
import asyncio
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='$', intents=intents)
 
@bot.event
async def on_ready():
    print(f'Estamos logados como {bot.user}')

@bot.command()
async def hello(ctx):
    await ctx.send(f'Olá! Eu sou o bot {bot.user}!')

@bot.command()
async def heh(ctx, count_heh=10):
    await ctx.send("he" * count_heh)

@bot.command()
async def charada(ctx):
    charadas = [
        ["O que é que tem coroa, mas não é rei e tem escama, mas não é peixe?", "abacaxi"],
        ["O que é que cai em pé e corre deitado?", "chuva"],
        ["O que é que nasce grande e morre pequeno?", "lápis"]
    ]
    
    pergunta, resposta = random.choice(charadas)
    await ctx.send(f'🤔 *Charada:* {pergunta}')

    def check(m):
        return m.author == ctx.author and m.channel == ctx.channel

    try:
        
        msg = await bot.wait_for('message', check=check, timeout=30.0)
        
        if resposta in msg.content.lower():
            await ctx.send(f'🎯 Na mosca! A resposta é *{resposta}*.')
        else:
            await ctx.send(f'Putz, não é isso! A resposta era: *{resposta}*.')
            
    except asyncio.TimeoutError:
        await ctx.send(f'⏰ Tempo esgotado! A resposta era: *{resposta}*.')

@bot.command()
async def meme_programação(ctx):
    image_meme = random.choice(os.listdir('images'))
    with open(f'images/{image_meme}', 'rb') as f:
        picture = discord.File(f)
    await ctx.send(file=picture)
@bot.command()
async def meme_aleatorio(ctx):
    image_meme = random.choice(os.listdir('memes_aleatórios'))
    with open(f'memes_aleatórios/{image_meme}', 'rb') as f:
        picture = discord.File(f)
    await ctx.send(file=picture)
@bot.command()
async def composisao_material(ctx):
    biodegradaveis = "Papel,madeira... papelão, bambu e etc."
    texto_educativo = "Você sabia que o plástico leva cerca de 450 anos para se decompor? O alumínio pode levar 500 anos e o vidro leva mais de 1000 anos para se decompor \n Vários e vários anos de lixo no meio ambiente... que tal usarmos mais materias biodegradáveis?"
    print(biodegradaveis)
    await ctx.send(texto_educativo)
    await ctx.send(f"*exemplos =)* {biodegradaveis}")


bot.run("tokem_aqui")
