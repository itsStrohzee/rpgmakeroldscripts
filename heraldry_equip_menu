```ruby
#===============================================================================
# ** Scene_Equip
#===============================================================================
class Scene_Equip < Scene_MenuBase
  
  def start
    super
    @actor = $game_party.menu_actor
    create_all_graphics
  end
  
  def create_all_graphics
    create_main_graphics
    create_fog
    create_stat_box
    create_equipment_box
    create_amp_box
    create_slots
    create_inventory_box
    create_stat_window
    create_equip_window
    create_amp_window
    create_sigium_window
    create_inventory_window
    create_help_window
    create_party_window
  end
  
  def create_party_window
    @party_bg = Sprite.new ; @party_bg.bitmap = Cache.menu_equip("EQ_PartyBar")
    @party_bg.x = 201      ; @party_bg.y = 77 ; @party_bg.z = 3
    @actor_icons = []
    $game_party.battle_members.each_with_index do |actor, index|
      icon = nil if icon
      icon = Sprite.new
      icon.bitmap = Cache.empty_bitmap
      icon.x = (@party_bg.x + 29) + (36 * index)
      icon.y = @party_bg.y + 1
      icon.z = 4
      icon.opacity = 0
      @actor_icons.push([icon, actor])
    end
    update_party_window
  end
  #-------------------------------
  # * Create Main Graphics
  #-------------------------------
  def create_main_graphics
    @main_background = Sprite.new
    @main_background.bitmap = Cache.menu_main("Background")
    @main_background.opacity = 225
    @main_help_bar = Sprite.new
    @main_help_bar.bitmap = Cache.menu_main("Help_Bar")
    @main_help_bar.blend_type = 2
    @main_help_bar.opacity = 160
    @equip_header = Sprite.new
    @equip_header.bitmap = Cache.menu_equip("EQ_Header")
    @equip_header.x = -24
    @equip_header.y = 26
  end
  
  def create_fog
    @main_fog = Plane.new
    @main_fog.bitmap = Cache.menu_main("Fog")
    @main_fog.z = 2
    @main_fog.opacity = 85
  end
  
  def create_stat_box
    @stat_box = Sprite.new
    @stat_box.bitmap = Cache.menu_equip("EQ_StatBox")
    @stat_box.x = 3
    @stat_box.y = 80
    @stat_box.z = 3
    @stat_box.opacity = 0
    @face_graphic = Sprite.new
    @face_graphic.x = 13
    @face_graphic.y = 63
    @face_graphic.z = 4
    update_face_graphic
  end
  
  def create_equipment_box
    @equip_box = Sprite.new
    @equip_box.bitmap = Cache.menu_equip("EQ_GearBox")
    @equip_box.x = 165
    @equip_box.y = 125
    @equip_box.opacity = 0
    @equip_box.z = 3
  end
  
  def create_amp_box
    @amp_box = Sprite.new
    @amp_box.bitmap = Cache.menu_equip("EQ_SigiumBox")
    @amp_box.x = 391
    @amp_box.y = 121
    @amp_box.opacity = 0
    @amp_box.z = 3
  end
  
  def create_slots(switch = false)
    if @amp1_slots
      @amp1_slots.each {|s| s.bitmap.dispose}
    end
    @amp1_slots = []
    if !@actor.equips[6].nil?
      slots = @actor.equips[6].sigium_slot
      slots.each_with_index do |slot, i|
        bmp = Sprite.new
        case slot
        when 0
          next
        when 1
          bmp.bitmap = Cache.menu_equip("Sigium_Diamond")
        when 2
          bmp.bitmap = Cache.menu_equip("Sigium_Square")
        when 3
          bmp.bitmap = Cache.menu_equip("Sigium_Triangle")
        end
        bmp.opacity = switch ? 255 : 0
        bmp.x = 402
        bmp.y = 193 + (24 * i)
        bmp.z = 4
        @amp1_slots.push(bmp)
      end
    elsif !@amp1_slots.empty?
      @amp1_slots.each {|s| s.bitmap.dispose}
      @amp1_slots = []
    end
  end
  
  def create_inventory_box
    @inventory_box         = Sprite.new
    @inventory_box.bitmap  = Cache.menu_equip("EQ_InventoryR")
    @inventory_box.x       = 370
    @inventory_box.y       = 69
    @inventory_box.z       = 20
    @inventory_box.opacity = 0
  end
  
  def create_stat_window
    @stat_window = Window_EquipStat.new(70,164,100,197,@actor)
    @stat_window.hide
  end
  
  def create_equip_window
    @equip_window = Window_EquipEquips.new(174,167,214,206,@actor)
    @equip_window.set_handler(:right, method(:on_cursor_right))
    @equip_window.set_handler(:ok,    method(:on_equip_ok))
    @equip_window.hide
    @equip_window.deactivate
    @equip_window.z = 19
  end
  
  def create_amp_window
    @amp_window = Window_EquipAmps.new(400,167,214,240,@actor)
    @amp_window.set_handler(:left, method(:on_cursor_left))
    @amp_window.set_handler(:ok,   method(:on_amp_ok))
    @amp_window.set_handler(:xbutton, method(:on_amp_xbutton))
    @amp_window.hide
    @amp_window.deactivate
    @amp_window.z = 19
  end
  
  def create_sigium_window
    @sig_window = Window_EquipSigium.new(405,194,184,72,@actor)
    @sig_window.set_handler(:ok,     method(:on_sigil_ok))
    @sig_window.set_handler(:cancel, method(:on_sigil_cancel))
    @sig_window.hide
    @sig_window.z = 19
  end
  
  def create_inventory_window
    @inventory_window = Window_EquipItem.new(384,98,210,184)
    @inventory_window.set_handler(:cancel, method(:on_inv_cancel))
    @inventory_window.set_handler(:ok,     method(:on_inv_ok))
    @inventory_window.z = 21
    @inventory_window.actor = @actor
    @inventory_window.contents_opacity = 0
    @equip_window.inventory_window = @inventory_window
    @amp_window.inventory_window = @inventory_window
    @sig_window.inventory_window = @inventory_window
    @inventory_window.status_window = @stat_window
  end
  
  def create_help_window
    @help_window = Window_EquipHelp.new(0,438,640,50)
    @inventory_window.help_window = @help_window
    @equip_window.help_window = @help_window
    @amp_window.help_window = @help_window
    @sig_window.help_window = @help_window
  end

  #-------------------------------------
  # * Right Cursor Input
  #-------------------------------------
  def on_cursor_right
    return if !@amp_window.visible
    @equip_window.unselect
    @equip_window.deactivate
    @amp_window.activate
    @amp_window.select(0)
    @inventory_box.x = 190
    @inventory_window.x = 195
    @inventory_box.opacity = 0
    @inventory_box.bitmap = Cache.menu_equip("EQ_InventoryL")
  end
  #-------------------------------------
  # * Left Cursor Input
  #-------------------------------------
  def on_cursor_left
    @amp_window.unselect
    @amp_window.deactivate
    @equip_window.activate
    @equip_window.select(0)
    @inventory_box.x = 370
    @inventory_window.x = 384
    @inventory_box.opacity = 0
    @inventory_box.bitmap = Cache.menu_equip("EQ_InventoryR")
  end
  
  def on_equip_ok
    return unless @inventory_window
    @equip_window.deactivate
    @active_window = :equip
    @inventory_window.show
    @inventory_window.activate
    @inventory_window.select(0)
    @inventory_window.update_help
  end
  
  def on_amp_ok
    return unless @inventory_window
    @amp_window.deactivate
    @active_window = :amp
    @inventory_window.show
    @inventory_window.activate
    @inventory_window.select(0)
    @inventory_window.update_help
  end
  
  def on_amp_xbutton
    return if !@amp_window.active
    if @actor.equips[6].nil?
      Sound.play_buzzer
    else
      Sound.play_ok
      @amp_window.deactivate
      @sig_window.activate
      @sig_window.select(0)
    end
  end
  
  def on_inv_cancel
    @inventory_window.hide
    @inventory_window.unselect
    @inventory_window.deactivate
    case @active_window
    when :sigil
      @sig_window.activate
      @stat_window.set_temp_actor(nil)
      @active_window = :amp
    when :equip
      @equip_window.activate
      @stat_window.set_temp_actor(nil)
    when :amp
      @amp_window.activate
      @stat_window.set_temp_actor(nil)
    end
  end
  
  def on_inv_ok
    Sound.play_equip
    if @active_window == :sigil
      @actor.change_equip(@sig_window.index + 7, @inventory_window.item)
      @sig_window.activate
      @sig_window.refresh
    elsif @active_window == :equip
      @actor.change_equip(@equip_window.index, @inventory_window.item)
      @equip_window.activate
      @equip_window.refresh
    elsif @active_window == :amp
      @actor.change_equip(@amp_window.index + 6, @inventory_window.item)
      @amp_window.activate
      @amp_window.refresh
      @sig_window.refresh
      @sig_window.rearrange_sigium
      create_slots
    end
    @inventory_window.unselect
    @inventory_window.refresh
    @inventory_window.hide
    @stat_window.set_temp_actor(nil)
    @stat_window.refresh
  end
  
  def on_sigil_ok
    return unless @inventory_window
    @sig_window.deactivate
    @active_window = :sigil
    @inventory_window.show
    @inventory_window.activate
    @inventory_window.select(0)
    @inventory_window.update_help
  end
  
  def on_sigil_cancel
    @sig_window.unselect
    @sig_window.deactivate
    @amp_window.activate
  end
    
  def update_face_graphic
    @face_graphic.bitmap = Cache.menu_equip("Actor"+@actor.id.to_s)
    @face_graphic.opacity = 0
    @face_graphic.x = -5
  end
  #-------------------------
  # * Update
  #-------------------------
  def update_basic
    @main_fog.ox -= 1
    update_graphics_opacity
    update_window_visible
    update_equip_window_active
    update_inventory_window
    if Input.trigger?(:B)
      $game_system.last_menu_index = 1
      $game_system.reorder_main = true
      Sound.play_cancel
      SceneManager.return
    end
    update_input if $game_party.battle_members.length > 1
    update_icons_opacity
    super
  end
  #------------------------------
  # * Update Visible Windows
  #------------------------------
  def update_window_visible
    @stat_window.show if @stat_box.opacity     >= 100
    @equip_window.show if @equip_box.opacity   >= 100
    @amp_window.show if @amp_box.opacity       >= 100
    @sig_window.show if @amp_box.opacity       >= 100
  end
  #------------------------------
  # * Activate Equip Window
  #------------------------------
  def update_equip_window_active
    if !@equip_window.active && @equip_box.opacity >= 100
      if !@amp_window.active && !@inventory_window.active && !@sig_window.active
        @equip_window.activate
        @equip_window.select(0)
      end
    end
  end
  #------------------------------
  # * Update Inventory
  #------------------------------
  def update_inventory_window
    
    if @active_window == :equip
      pos_y = @equip_window.index * 36
      if @inventory_window.active && @inventory_box.x < 385
        @inventory_box.y  = 69 + pos_y
        @inventory_window.y = 98 + pos_y
        @inventory_box.x       += 1 unless @inventory_box.x >= 385
        @inventory_window.x    += 1 unless @inventory_window.x >= 399
        @inventory_box.opacity += 25
        @inventory_window.contents_opacity += 25
      elsif !@inventory_window.active && @inventory_box.x > 370
        @inventory_box.x       -= 1 unless @inventory_box.x <= 370
        @inventory_window.x    -= 1 unless @inventory_window.x <= 385
        @inventory_box.opacity -= 25
        @inventory_window.contents_opacity -= 25
      end
      
    elsif @active_window == :amp
      pos_y = @amp_window.index * 36
      if @inventory_window.active && @inventory_box.x > 175
        @inventory_box.y = 69 + pos_y
        @inventory_window.y = 98 + pos_y
        @inventory_box.x -= 1 unless @inventory_box.x <= 175
        @inventory_window.x    -= 1 unless @inventory_window.x <= 180
        @inventory_window.contents_opacity += 25
        @inventory_box.opacity += 25
      elsif !@inventory_window.active && @inventory_box.x < 190
        @inventory_box.x    += 1 unless @inventory_box.x    >= 190
        @inventory_window.x += 1 unless @inventory_window.x >= 199
        @inventory_box.opacity -= 25
        @inventory_window.contents_opacity -= 25
      end
      
    elsif @active_window == :sigil
      pos_y = @sig_window.index * 20
      if @inventory_window.active && @inventory_box.x > 175
        @inventory_box.y = 98 + pos_y
        @inventory_window.y = 127 + pos_y
        @inventory_box.x -= 1 unless @inventory_box.x <= 175
        @inventory_window.x    -= 1 unless @inventory_window.x <= 180
        @inventory_box.opacity += 25
        @inventory_window.contents_opacity += 25
      elsif !@inventory_window.active && @inventory_box.x < 190
        @inventory_box.x += 1 unless @inventory_box.x >= 190
        @inventory_box.opacity -= 25
        @inventory_window.x += 1 unless @inventory_window.x >= 199
        @inventory_window.contents_opacity -= 25
      end
    end
    
  end
  #------------------------------
  # * Update Opacity
  #------------------------------
  def update_graphics_opacity
    @stat_box.opacity      += 15 unless @stat_box.opacity      >= 255
    @equip_box.opacity     += 15 unless @equip_box.opacity     >= 255
    @amp_box.opacity       += 15 unless @amp_box.opacity       >= 255
    if @face_graphic.opacity < 255 && @stat_box.opacity >= 100
      @face_graphic.opacity += 25 unless @face_graphic.opacity >= 255
      @face_graphic.x += 1 unless @face_graphic.x >= 8
    end
    if !@amp1_slots.empty?
      @amp1_slots.each {|s| s.opacity += 15 unless s.opacity >= 255}
    end
  end
  #------------------------------
  # * Check L+R
  #------------------------------
  def update_input
    return if @inventory_window.active
    if Input.trigger?(:R)
      Audio.se_play('Audio/SE/Menu_Change',80,90)
      @actor = $game_party.menu_actor_next
      update_face_graphic
      @equip_window.actor = @actor
      @equip_window.refresh ; @equip_window.update_help if @equip_window.active
      @amp_window.actor = @actor
      @amp_window.refresh   ; @amp_window.update_help if @amp_window.active
      @sig_window.actor = @actor
      @amp_window.refresh   ; @amp_window.update_help if @amp_window.active
      @stat_window.actor = @actor
      if @sig_window.active
        @sig_window.deactivate
        @sig_window.unselect
        @amp_window.activate
      end
      @stat_window.refresh
      @inventory_window.actor = @actor
      @inventory_window.refresh
      update_party_window
      create_slots(true)
    end
    if Input.trigger?(:L)
      Audio.se_play('Audio/SE/Menu_Change',80,90)
      @actor = $game_party.menu_actor_prev
      update_face_graphic
      @equip_window.actor = @actor
      @equip_window.refresh ; @equip_window.update_help if @equip_window.active
      @amp_window.actor = @actor
      @amp_window.refresh   ; @amp_window.update_help if @amp_window.active
      @sig_window.actor = @actor
      @amp_window.refresh   ; @amp_window.update_help if @amp_window.active
      @stat_window.actor = @actor
      if @sig_window.active
        @sig_window.deactivate
        @sig_window.unselect
        @amp_window.activate
      end
      @stat_window.refresh
      @inventory_window.actor = @actor
      @inventory_window.refresh
      update_party_window
      create_slots(true)
    end
  end
  
  def update_party_window
    return unless @actor_icons
    rect = Rect.new(32,0,32,32)
    @actor_icons.each {|icon, actor| icon.bitmap.clear}
    @actor_icons.each do |icon, actor|
      if actor.id == @actor.id
        icon.bitmap = Cache.menu_equip("Actor_Active").dup
        opacity = 255
      else
        icon.bitmap = Cache.menu_equip("Actor_Inactive").dup
        opacity = 200
      end
      #This is really dumb but necessary
      plus_y = ( actor.id == 9 ? 1 : 0 )
      #==================================
      act_bmp = Cache.battler(actor.name + "_1", 0)
      icon.bitmap.blt(4, 2 - plus_y, act_bmp, rect, opacity)
    end
    update_icons_opacity
  end
  
  def update_icons_opacity
    return unless @actor_icons
    return unless @actor_icons[0][0].bitmap
    return if @actor_icons.last[0].opacity >= 255
    @actor_icons.each do |icon, actor|
      next if icon.opacity >= 255
      icon.opacity += 15
    end
  end
  
  def dispose_actor_icons
    @actor_icons.each {|icon, actor| icon.bitmap.dispose ; icon.dispose}
    @actor_icons.clear
  end
        
  def terminate
    super
    @stat_window.dispose
    @amp_window.dispose
    @main_background.bitmap.dispose ; @main_background.dispose
    @equip_header.bitmap.dispose
    @equip_header.dispose
    @main_fog.bitmap.dispose
    @main_fog.dispose
    @stat_box.bitmap.dispose
    @stat_box.dispose
    @face_graphic.bitmap.dispose
    @face_graphic.dispose
    @equip_box.bitmap.dispose
    @equip_box.dispose
    @amp_box.bitmap.dispose
    @amp_box.dispose
    @amp1_slots.each {|s| s.bitmap.dispose ; s.dispose}
    @inventory_box.bitmap.dispose
    @inventory_box.dispose
    @party_bg.bitmap.dispose
    @party_bg.dispose
    dispose_actor_icons
  end
  
end
#===============================================================================
# ** Window_EquipStat
#===============================================================================
class Window_EquipStat < Window_Base
  
  def initialize(x,y,width,height,actor)
    super(x,y,width,height)
    self.windowskin = Cache.system("Window2")
    @actor = actor
    refresh
  end
  
  def standard_padding ; 0 ; end
  
  def actor=(actor)
    @actor = actor
    refresh
  end
  
  def set_temp_actor(temp_actor)
    return if @temp_actor == temp_actor
    @temp_actor = temp_actor
    refresh
  end
  
  def refresh
    a = @actor
    contents.font.name = "FOT-Chiaro Std B"
    contents.font.size = 13
    contents.font.outline = false
    contents.font.shadow = false
    contents.clear
    draw_img_number(a.level.to_s, 0, 3 - 1)
    draw_img_number(a.mhp.to_s,   0, 20 - 1)
    draw_img_number(a.mmp.to_s,   0, 36 - 1)
    draw_img_number(a.atk.to_s,   0, 52 - 1)
    draw_img_number(a.mat.to_s,   0, 68 - 1)
    draw_img_number(a.def.to_s,   0, 84 - 1)
    draw_img_number(a.mdf.to_s,   0, 100 - 1)
    draw_img_number(a.luk.to_s,   0, 116 - 1)
    draw_img_number(a.agi.to_s,   0, 132 - 1)
#~     draw_text(0,143,38,11,(a.hit * 100).to_s)
#~     draw_text(0,156,38,11,(a.eva * 100).to_s)
#~     draw_text(0,169,38,11,(a.cri * 100).to_s)
#~     draw_text(0,182,38,11,(a.cnt * 100).to_s)
    refresh_with_temp if @temp_actor
  end
  
  def refresh_with_temp
    actor = @actor
    temp = @temp_actor
    draw_diff(40,19,actor.mhp,temp.mhp) if actor.mhp != temp.mhp
    draw_diff(40,35,actor.mmp,temp.mmp) if actor.mmp != temp.mmp
    draw_diff(40,51,actor.atk,temp.atk) if actor.atk != temp.atk
    draw_diff(40,67,actor.mat,temp.mat) if actor.mat != temp.mat
    draw_diff(40,83,actor.def,temp.def) if actor.def != temp.def
    draw_diff(40,99,actor.mdf,temp.mdf) if actor.mdf != temp.mdf
    draw_diff(40,115,actor.luk,temp.luk) if actor.luk != temp.luk
    draw_diff(40,131,actor.agi,temp.agi) if actor.agi != temp.agi
    draw_diff(40,143,actor.hit * 100,temp.hit * 100) if actor.hit != temp.hit
    draw_diff(40,156,actor.eva * 100,temp.eva * 100) if actor.eva != temp.eva
    draw_diff(40,169,actor.cri * 100,temp.cri * 100) if actor.cri != temp.cri
    draw_diff(40,182,actor.cnt * 100,temp.cnt * 100) if actor.cnt != temp.cnt
  end
    
  def draw_diff(x,y,param1,param2)
    diff = param2 - param1
    if diff < 0
      type = :minus
      sym = "-"
    else
      type = :plus
      sym = "+"
    end
    diff = diff.floor
    draw_img_number(sym + diff.abs.to_s, x, y, type)
  end
  
end
#===============================================================================
# ** Window_EquipEquips
#===============================================================================  
class Window_EquipEquips < Window_Selectable
  
  def initialize(x,y,width,height,actor)
    super(x,y,width,220)
    @actor = actor
    self.windowskin = Cache.system("Window2")
    refresh
  end
  
  def inventory_window=(window) ; @inventory_window = window ; end
  def window_index=(index) ; @window_index = index ; end
  def actor=(actor) ; @actor = actor ; end
  def standard_padding ;    return 0 ; end
  def item_max ;            return 6 ; end
  def visible_line_number ; return 6 ; end
  def line_height;          return 39 ; end
  
  def item
    @actor ? @actor.equips[index] : nil
  end
  
  def update
    super
    @inventory_window.slot_id = index if @inventory_window && self.active
  end
  
  def update_help
    return if !@help_window
    @help_window.set_item(@actor.equips[index])
  end
  
  def draw_nothing_equip(dx, dy, enabled, dw)
    contents.font.outline = true
    contents.font.shadow  = true
    change_color(normal_color, enabled)
    text = "[Empty]"
    draw_text(dx + 24, dy, dw - 24, line_height, text)
  end
  
  def draw_item(index)
    return unless @actor
    return if index > 5
    rect = item_rect_for_text(index)
    item = @actor.equips[index]
    dx = rect.x
    dw = contents.width - dx - 24
    self.contents.font.name = "FOT-Chiaro Std B"
    self.contents.font.size = 17
    if index != 5
      if item.nil?
        draw_nothing_equip(dx + 6, rect.y - 7, false, dw)
      else
        draw_item_name(item, dx, rect.y, true, dw)
      end
    else
      if item.nil?
        draw_nothing_equip(dx + 6, rect.y - 7, false, dw)
      else
        draw_item_name(item, dx, rect.y, true, dw)
      end
    end
  end
    
  def draw_item_name(item, x, y, enabled = true, width = 172)
    return unless item
    contents.font.outline = true
    contents.font.shadow  = true
    draw_icon(item.icon_index, x - 1, y, enabled)
    change_color(normal_color, enabled)
    draw_text(x + 30, y + 2, width, 24, item.name)
  end
  
  def item_height
    24
  end
  
  def cursor_right(wrap = false)
    call_handler(:right)
  end

  def item_rect(index)
    rect = Rect.new
    rect.width = item_width
    rect.height = 24
    rect.x = (index % col_max * (item_width + spacing))
    rect.y = index * 36
    rect
  end
  
end
#===============================================================================
# ** Window_EquipAmps
#===============================================================================  
class Window_EquipAmps < Window_Selectable
  
  def initialize(x,y,width,height,actor)
    super(x,y,width,height)
    @actor = actor
    self.windowskin = Cache.system("Window2")
    refresh
  end
  
  def inventory_window=(window) ; @inventory_window = window ; end
  def window_index=(index) ; @window_index = index ; end
  def actor=(actor) ; @actor = actor ; end
  def standard_padding ;    return 0 ; end
  def item_max ; $game_switches[100] ? 2 : 1 ; end
  def visible_line_number ; return 2 ; end
  def line_height;          return 116 ; end
  
  def item
    @actor ? @actor.equips[index] : nil
  end
  
  def update
    super
    @inventory_window.slot_id = (index + 6) if @inventory_window && self.active
  end
  
  def update_help
    return if !@help_window
    @help_window.set_item(@actor.equips[index + 6])
  end
  
  def draw_nothing_equip(dx, dy, enabled, dw)
    change_color(normal_color, enabled)
    text = "[Empty]"
    draw_text(dx + 30, dy - 42, dw - 24, line_height, text)
  end
  
  def draw_item(index)
    return unless @actor
    return if index == 1 && !$game_switches[100]
    return if index > 1
    rect = item_rect_for_text(index)
    item = @actor.equips[index + 6]
    dx = rect.x
    dw = contents.width - dx - 24
    self.contents.font.name = "FOT-Chiaro Std B"
    self.contents.font.size = 17
    if item.nil?
      draw_nothing_equip(dx, rect.y, false, dw)
    else
      draw_item_name(item, dx, rect.y, true, dw)
    end
  end
    
  def draw_item_name(item, x, y, enabled = true, width = 172)
    return unless item
    contents.font.outline = true
    contents.font.shadow  = true
    draw_icon(item.icon_index, x - 1, y, enabled)
    change_color(normal_color, enabled)
    draw_text(x + 30, y + 3, width, 24, item.name)
  end
  
  def item_height ; 24 ; end
  
  def cursor_left(wrap = false)
    call_handler(:left)
  end
  
  def process_xbutton
    call_handler(:xbutton)
  end

  def item_rect(index)
    rect = Rect.new
    rect.width = item_width
    rect.height = 24
    rect.x = 0 #(index % col_max * (item_width + spacing))
    rect.y = 36 * index
    rect
  end
  
  def process_handling
    super
    return process_xbutton  if handle?(:xbutton)  && Input.trigger?(:Z)
  end
  
end
#===============================================================================
# ** Window_EquipSigium
#=============================================================================== 
class Window_EquipSigium < Window_Selectable
  
  def initialize(x,y,width,height,actor)
    super(x,y,width,height)
    @actor = actor
    self.windowskin = Cache.system("Window2")
    refresh
  end
  
  def standard_padding ; 0 ; end
    
  def item_max
    return 0 if !@actor
    return 0 if @actor.equips[6].nil?
    array = @actor.equips[6].sigium_slot.dup
    array.delete(0)
    array.length
  end
  
  def visible_line_number ; return 3 ; end
  def line_height;          return 24 ; end
  
  def actor=(actor)
    @actor = actor
    refresh
  end
  
  def inventory_window=(inventory_window)
    @inventory_window = inventory_window
  end
  
  def update
    super
    @inventory_window.slot_id = (index + 7) if @inventory_window && self.active
  end
  
  def update_help
    return if !@help_window
    @help_window.set_item(@actor.equips[index + 7])
  end
  
  def rearrange_sigium
    i = 0
    if @actor.equips[6].nil?
      @actor.change_equip(7, nil)
      @actor.change_equip(8, nil)
      @actor.change_equip(9, nil)
      return
    end
    new_amp_sigium = @actor.equips[6].sigium_slot
    array = Array.new(3)
    array = [@actor.equips[7],@actor.equips[8],@actor.equips[9]]
    array.each do |slot|
      if slot.nil?
        i += 1
        next
      elsif slot.sigium_type.to_i != new_amp_sigium[array.index(slot)].to_i
        @actor.change_equip(i + 7, nil)
      end
      i += 1
    end
    refresh
  end
      
  def draw_item(index)
    return unless @actor
    return if index > 2
    rect = item_rect_for_text(index)
    item = @actor.equips[index + 7]
    dx = rect.x
    dw = contents.width - dx - 24
    self.contents.font.name = "FOT-Chiaro Std B"
    self.contents.font.size = 17
    self.contents.font.outline = true
    self.contents.font.shadow = true
    if item.nil?
      draw_nothing_equip(rect.x, rect.y - 1, false, dw)
    else
      change_color(normal_color, true)
      draw_icon(item.icon_index, rect.x - 30, rect.y - 2)
      draw_text(rect.x, rect.y + 2, rect.width, rect.height, item.name)
    end
  end
  
  def draw_nothing_equip(dx, dy, enabled, dw)
    change_color(normal_color, enabled)
    text = "[Empty]"
    draw_text(dx, dy + 1, dw - 24, line_height, text)
  end
  
  def item_rect(index)
    rect = Rect.new
    rect.width = 154
    rect.height = 20
    rect.x = (index % col_max * (item_width + spacing)) + 25
    rect.y = index / col_max * line_height + 2
    rect
  end
  
end
#===============================================================================
# ** Window_EquipItem
#=============================================================================== 
class Window_EquipItem < Window_ItemList
  
  def include?(item)
    return true if item == nil && @slot_id > 1
    return false unless item.is_a?(RPG::EquipItem)
    return false if @slot_id < 0
    return false if item.etype_id != @actor.equip_slots[@slot_id]
    if item.sigium_type != 0 || nil
      return false if @actor.equips[6].nil?
      return false if item.sigium_type != @actor.equips[6].sigium_slot[@slot_id - 7]
    end
    return false if @slot_id == 0 && item.secondary?
    return false if @slot_id == 1 && !item.secondary?
    return @actor.equippable?(item)
  end
  
end
#===============================================================================
# ** Window_EquipHelp
#=============================================================================== 
class Window_EquipHelp < Window_Base
  
  def initialize(x, y, width, height)
    super(x, y ,width, height)
    self.windowskin = Cache.system("Window2")
    self.contents.font.name = "FOT-Chiaro Std B"
    self.contents.font.size = 17
    self.contents.font.outline = false
    self.contents.font.shadow = true
  end
  
  def set_item(item)
    if item != @item
      @item = item
      refresh
    end
  end
  
  def clear
    @item = nil
    contents.clear
  end
  
  def standard_padding ; 0 ; end
  def line_height ; 16 ; end
  
  def refresh
    contents.clear
    return unless @item
    change_color(text_color(10))
    draw_text(4,0,640,32,"》" + @item.name,0)
    change_color(normal_color)
    draw_text_ex(4,22,@item.description)
  end
  
  def draw_text_ex(x, y, text)
    text = convert_escape_characters(text)
    pos = {:x => x, :y => y, :new_x => x, :height => calc_line_height(text)}
    process_character(text.slice!(0, 1), text, pos) until text.empty?
  end
  
end
```
