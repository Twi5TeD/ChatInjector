package com.extendedclip.chatinjector;

import com.comphenix.protocol.ProtocolLibrary;
import com.comphenix.protocol.ProtocolManager;
import com.comphenix.protocol.events.ListenerPriority;
import com.comphenix.protocol.events.PacketAdapter;
import com.comphenix.protocol.events.PacketContainer;
import com.comphenix.protocol.events.PacketEvent;
import com.comphenix.protocol.reflect.StructureModifier;
import com.comphenix.protocol.wrappers.WrappedChatComponent;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import me.clip.placeholderapi.PlaceholderAPI;
import me.clip.placeholderapi.PlaceholderAPIPlugin;
import net.md_5.bungee.api.chat.BaseComponent;
import net.md_5.bungee.chat.ComponentSerializer;

public class SpigotChatPacketListener extends PacketAdapter
{
  public SpigotChatPacketListener()
  {
    super(PlaceholderAPIPlugin.getInstance(), ListenerPriority.HIGHEST, new com.comphenix.protocol.PacketType[] { com.comphenix.protocol.PacketType.Play.Server.CHAT });
    ProtocolLibrary.getProtocolManager().addPacketListener(this);
  }
  
  public void onPacketSending(PacketEvent e)
  {
    if (e.getPlayer() == null) {
      return;
    }
    
    StructureModifier<WrappedChatComponent> chat = e.getPacket().getChatComponents();
    
    WrappedChatComponent c = (WrappedChatComponent)chat.read(0);
    
    if (c == null)
    {
      StructureModifier<BaseComponent[]> modifier = e.getPacket().getSpecificModifier(BaseComponent[].class);
      
      BaseComponent[] components = (BaseComponent[])modifier.readSafely(0);
      
      if (components == null) {
        return;
      }
      
      String msg = ComponentSerializer.toString(components);
      
      if (msg == null) {
        return;
      }
      
      if (!PlaceholderAPI.getBracketPlaceholderPattern().matcher(msg).find()) {
        return;
      }
      
      msg = PlaceholderAPI.setBracketPlaceholders(e.getPlayer(), msg);
      
      modifier.write(0, ComponentSerializer.parse(msg));
      
      return;
    }
    
    String msg = c.getJson();
    
    if (msg == null) {
      return;
    }
    
    if (!PlaceholderAPI.getBracketPlaceholderPattern().matcher(msg).find()) {
      return;
    }
    
    msg = PlaceholderAPI.setBracketPlaceholders(e.getPlayer(), msg);
    
    chat.write(0, WrappedChatComponent.fromJson(msg));
  }
}
