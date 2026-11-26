# Discord-server
Discord 
@app_commands.command(name="help")
@app_commands.default_permissions(send_messages=True)
async def help(interaction: discord.Interaction):
    await interaction.response.send_message("Help menu", ephemeral=True)
