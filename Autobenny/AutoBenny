using Dalamud.Game.Command;
using Dalamud.Plugin;
using Dalamud.Game.ClientState.Actors.Types;
using System.Linq;
using Dalamud.Game.ClientState.Actors.Types.NonPlayer;
using Dalamud.Game.ClientState.Party;

namespace AutoBenny
{
    public class AutoBennyPlugin : IDalamudPlugin
    {
        public string Name => "Auto Benny";

        private const string CommandName = "/autobenny";
        private const string HealthCommandName = "/bennyhealth";
        private readonly DalamudPluginInterface _pluginInterface;
        private readonly CommandManager _commandManager;
        private readonly Framework _framework;
        private readonly PartyList _partyList;

        private bool _enabled = false;
        private const int BenedictionActionId = 140;
        private float _hpThreshold = 0.2f;

        public AutoBennyPlugin(
            DalamudPluginInterface pluginInterface,
            CommandManager commandManager,
            Framework framework,
            PartyList partyList)
        {
            _pluginInterface = pluginInterface;
            _commandManager = commandManager;
            _framework = framework;
            _partyList = partyList;

            _commandManager.AddHandler(CommandName, new CommandInfo(OnCommand)
            {
                HelpMessage = "Toggles the Auto Benny plugin on/off."
            });

            _commandManager.AddHandler(HealthCommandName, new CommandInfo(OnHealthCommand)
            {
                HelpMessage = "Set the HP threshold for Auto Benny (0-100)."
            });

            _framework.Update += Update;
        }

        public void Dispose()
        {
            _framework.Update -= Update;
            _commandManager.RemoveHandler(CommandName);
            _commandManager.RemoveHandler(HealthCommandName);
        }

        private void OnCommand(string command, string args)
        {
            _enabled = !_enabled;
            _pluginInterface.Framework.Gui.Chat.Print($"Auto Benny is now {(_enabled ? "enabled" : "disabled")}.");
        }

        private void OnHealthCommand(string command, string args)
        {
            if (int.TryParse(args, out int threshold) && threshold >= 0 && threshold <= 100)
            {
                _hpThreshold = (float)threshold / 100;
                _pluginInterface.Framework.Gui.Chat.Print($"Auto Benny HP threshold set to {threshold}%.");
            }
            else
            {
                _pluginInterface.Framework.Gui.Chat.Print("Invalid input. Please enter a value between 0 and 100.");
            }
        }

        private void Update()
        {
            if (!_enabled)
                return;

            CheckPartyMembersHealth();
        }

        private void CheckPartyMembersHealth()
        {
            var localPlayer = _pluginInterface.ClientState.LocalPlayer;
            if (localPlayer == null || localPlayer.ClassJob.Id != 24) // 24 is the White Mage's ClassJob Id
                return;

            foreach (var member in _partyList)
            {
                if (member.IsMe)
                    continue;

                if (member.CurrentHp <= 0 || member.MaxHp <= 0)
                    continue;

                float hpPercentage = (float)member.CurrentHp / member.MaxHp;

                if (hpPercentage < _hpThreshold)
                {
                    var target = _pluginInterface.ClientState.Actors
                        .OfType<Character>()
                        .FirstOrDefault(actor => actor.ObjectId == member.ObjectId);

                    if (target != null)
                    {
                        localPlayer.TargetManager.SetTarget(target);
                        _pluginInterface.Framework.Gui.PvPActions.Execute(BenedictionActionId, target);
                        break;
                    }
                }
            }
        }
    }
}
