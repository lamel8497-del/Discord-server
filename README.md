# Discord-server
Discord 
import discord
from discord import app_commands
from discord.ext import commands


class HelpCog(commands.Cog):
    def __init__(self, bot: commands.Bot):
        self.bot = bot

    @app_commands.command(name="help", description="List available slash commands")
    @app_commands.describe(command="Optional command name")
    async def help(
        self,
        interaction: discord.Interaction,
        command: str | None = None,
    ):
        commands_by_name = {
            cmd.name: cmd
            for cmd in self.bot.tree.walk_commands()
            if isinstance(cmd, app_commands.Command)
        }

        if command:
            target = commands_by_name.get(command.lstrip("/").lower())
            if target is None:
                await interaction.response.send_message(
                    "That command was not found.",
                    ephemeral=True,
                )
                return

            await interaction.response.send_message(
                f"`/{target.name}` — {target.description or 'No description'}",
                ephemeral=True,
            )
            return

        lines = [
            f"`/{cmd.name}` — {cmd.description or 'No description'}"
            for cmd in sorted(commands_by_name.values(), key=lambda c: c.name)
        ]

        await interaction.response.send_message(
            "\n".join(lines) or "No slash commands available.",
            ephemeral=True,
        )


async def setup(bot: commands.Bot):
    await bot.add_cog(HelpCog(bot))
