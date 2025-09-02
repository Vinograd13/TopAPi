<?php
namespace beretezzka;

use pocketmine\plugin\PluginBase;
use pocketmine\event\Listener;
use pocketmine\block\Block;
use pocketmine\event\block\BlockPlaceEvent;
use pocketmine\event\player\PlayerJoinEvent;
use pocketmine\event\player\PlayerChatEvent;
use pocketmine\event\player\PlayerQuitEvent;
use pocketmine\event\entity\EntityDamageEvent;
use pocketmine\event\player\PlayerItemHeldEvent;
use pocketmine\event\player\PlayerInteractEvent;
use pocketmine\event\player\PlayerRespawnEvent;
use pocketmine\command\Command;
use pocketmine\command\CommandSender;
use pocketmine\command\ConsoleCommandSender;
use pocketmine\utils\Config;
use pocketmine\Server;
use pocketmine\Player;
use pocketmine\level\particle\FloatingTextParticle;
use pocketmine\math\Vector3;
use pocketmine\item\{Item, Elytra};
use pocketmine\entity\Effect;
use pocketmine\event\player\PlayerCommandPreprocessEvent;
use pocketmine\event\player\PlayerItemConsumeEvent;
use pocketmine\event\player\PlayerMoveEvent;
use pocketmine\event\player\PlayerDeathEvent;
use pocketmine\entity\Entity;
use pocketmine\level\Level;
use pocketmine\level\Location;
use pocketmine\event\entity\EntityDeathEvent;
use pocketmine\level\Explosion;
use pocketmine\level\Position;
use pocketmine\event\block\BlockBreakEvent;
use pocketmine\event\entity\EntityExplodeEvent;
use pocketmine\scheduler\Task;
use pocketmine\scheduler\CallbackTask;
use pocketmine\utils\Utils;
use pocketmine\event\command\CommandEvent;
use pocketmine\level\sound\{AnvilFallSound, AnvilUseSound, BlazeShopSound, BlockPlaceSound, EndermanTeleportSound, ExplodeSound, TNTPrimeSound};








class api extends PluginBase implements Listener {
	private $dev = false;
	   public $cmdblock;
       public $izumrud;
       public $almaz;
       public $mayak;
      private $lastMessageTime = [];
       
     private $runeEffectActive = [];
       public $task;
       public $hi;
       public $vaiptext;
       public $vaiped;
       private $end;
       private $end1;
       private $endMIN;
       private $endMAX;
       public $cfg;
       public $restart = 90 * 60;
       
    public function onEnable() {
    
    $vaipTime= new \DateTime('2025-01-30');
            $date = new \DateTime();
            $interval = $vaipTime->diff($date);

            $this->vaiped = sprintf(
                $vaipTime->format('Y-m-d'),
                $interval->days,
                $interval->h,
                $interval->i,
                $interval->s
            );
    
       $this->rtpfly = new FloatingTextParticle(new Vector3(191 + 0.5, 73 + 2, 167), "§6§l              [нᴀчᴀᴛь иᴦᴩу]
   §l§fᴨᴩыᴦᴀй ʙниз дᴧя ᴛᴇᴧᴇᴨоᴩᴛᴀции ʙ 
   оᴄноʙной ʍиᴩ и нᴀчни ᴨуᴛᴇɯᴇᴄᴛʙиᴇ!
   
   Пропиши комманду /rtp или прыгни в портал!");
   
    $this->vaiptext = new FloatingTextParticle(new Vector3(176, 75 + 0.9, 196+0.5), "До вайпа $interval->days Дней и $interval->h Часов. !");
    $this->almaz = new FloatingTextParticle(new Vector3(168, 72 + 0.9, 165+0.5), "almaz text");
    $this->mayak = new FloatingTextParticle(new Vector3(168, 72 + 0.9, 169+0.5), "mayak text");
    $this->izumrud = new FloatingTextParticle(new Vector3(168, 72 + 0.9, 173+0.5), "mayak text");
   
   
   $this->hi = new FloatingTextParticle(new Vector3(190 + 0.5, 77, 197), "§l§fДобро пожаловать на BeretMine!§r
   §f§lДуо Анархия (§7Сервер 201)§f
  §f§oПривет, смельчак! Готов бросить вызов этому серверу?
  §fНа §6BeretMine §fтебя ждёт полная свобода действий!
     §f§rсражайся!, строй!, взрывай!.
    §f§oПрисоединяйся или создай свою историю!
      §fСервер открыт для тебя!§f
   §l§f    Удачного развития! ");
        $this->end = new Vector3(71, 46, 151);
        $this->end1 = new Vector3(74, 46, 146);
       $this->endMIN = new Vector3(4858, 0, 2016);
        $this->endMAX = new Vector3(5021, 256, 1852);
        
        $this->getServer()->getPluginManager()->getPlugin("Amitisti");
        $this->getServer()->getPluginManager()->registerEvents($this, $this);
    	$this->task = $this->getScheduler()->scheduleRepeatingTask(new CallBackTask([$this,"restart"]),20);
    }

/*  public function onPlayerMove(PlayerMoveEvent $event) {
        $player = $event->getPlayer();

        $playerPosition = $player->getPosition();
        if($player->getLevel()->getName() == 'ender' && $player->getAllowFlight(true) && $player->getGamemode() !== 1){
          $player->setAllowFlight(false);
          $player->setFlying(false);
        }
        if($player->getLevel()->getName() == 'swat' && $player->getAllowFlight(true) && $player->getGamemode() !== 1){
          $player->setAllowFlight(false);
          $player->setFlying(false);
        } 
        if ($this->isInArea($playerPosition)) {
            $player->teleport(new Position(4992.50, 27.50, 1967.69, $this->getServer()->getLevelByName("ender")));
            $player->sendTitle("§f§lＥ§6Ｎ§fＤ");
    }
   }
  private function isInArea(Vector3 $position): bool {
    $minX = min($this->end->x, $this->end1->x);
     $maxX = max($this->end->x, $this->end1->x);
   $minY = min($this->end->y, $this->end1->y);
      $maxY = max($this->end->y, $this->end1->y);
  $minZ = min($this->end->z, $this->end1->z);
      $maxZ = max($this->end->z, $this->end1->z);


        return $position->x >= $minX && $position->x <= $maxX &&
            $position->z >= $minZ && $position->z <= $maxZ;
    } 
   
   */
   
   
   
  /* 
   private function enderMir(Vector3 $position): bool {
    $minX = min($this->endMIN->x, $this->endMAX->x);
     $maxX = max($this->endMIN->x, $this->endMAX->x);
   $minY = min($this->endMIN->y, $this->endMAX->y);
      $maxY = max($this->endMIN->y, $this->endMAX->y);
  $minZ = min($this->endMIN->z, $this->endMAX->z);
      $maxZ = max($this->endMIN->z, $this->endMAX->z);


        return $position->x >= $minX && $position->x <= $maxX &&
            $position->z >= $minZ && $position->z <= $maxZ;
    } 
    */
   /*
|‾‾‾‾‾\
|    /
|‾‾‾‾\                            
|    \  
|     \
                                         
   
|            |
|            |
|            |
|            |
 \________/
 
 
|\        |  
| \       |  
|  \      |  
|   \     |  
|     \  |  
 
 
 
|‾‾‾‾‾‾
|      
|‾‾‾‾‾‾
|      
|________
 
   _________
 /     _____| 
|    (____
 \___      \ 
  ____)     |
 |_____   / 
 
 
 
          |
          |
          √
 
 */
		  public function onMove(PlayerMoveEvent $event) {
			
    $player = $event->getPlayer();
            $hasElytra = false;
            $x = $player->getX();
            $y = $player->getY();
            $z = $player->getZ();
            if($player->getAllowFlight() && $y > 100 && $player->getGamemode() !== 1){
            	$event->setCancelled(); 
                $player->sendTitle("§l§fОпастно!");
            }
    foreach($player->getInventory()->getContents() as $item){
      $name = $item->getCustomName();
      if ($item instanceof Elytra) {
                $hasElytra = true;
            }
            if ($hasElytra && $player->getAllowFlight()) {
            	if($player->getGamemode(1)){
            	return;
            }
            $player->setAllowFlight(false);
            $player->setFlying(false);
        } elseif (!$hasElytra && $player->getAllowFlight()) {
            return true;
        }
      if($name == "§l§f§oᴩунᴀ диᴋᴀᴩя\n§o§fＢｅｒｅｔ§6Ｍｉｎｅ"){
       $player->addEffect(Effect::getEffect(Effect::BLINDNESS)->setDuration(200)->setAmplifier(1));
        $player->addEffect(Effect::getEffect(Effect::STRENGTH)->setDuration(200)->setAmplifier(2));
        $player->addEffect(Effect::getEffect(Effect::SPEED)->setDuration(200)->setAmplifier(2));
      }
      if($name == "§l§f§l§f§oᴩунᴀ бᴇᴩᴇᴛᴀ\n§o§fＢｅｒｅｔＭｉｎｅ"){
    $player->addEffect(Effect::getEffect(Effect::STRENGTH)->setDuration(200)->setAmplifier(1));
        $player->addEffect(Effect::getEffect(Effect::SPEED)->setDuration(200)->setAmplifier(3));
      }
            if($name == "§l§f§oᴩунᴀ ᴩᴇᴦᴇниᴩᴀции\n§r§fＢｅｒｅｔＭｉｎｅ"){
           $player->addEffect(Effect::getEffect(Effect::REGENERATION)->setDuration(200)->setAmplifier(0));
      }
      if ($item->getId() === 397 && $name === "§l§f§oбоɯᴋᴀ ᴀбᴄоᴧюᴛноᴦо") {
            	$player->addEffect(Effect::getEffect(Effect::SPEED)->setDuration(200)->setAmplifier(1));
            $player->addEffect(Effect::getEffect(17)->setDuration(200)->setAmplifier(3));
                $player->addEffect(Effect::getEffect(8)->setDuration(200)->setAmplifier(1));
                $player->addEffect(Effect::getEffect(Effect::STRENGTH)->setDuration(200)->setAmplifier(2));
                
            }
      if($name == "§l§fᴩунᴀ ᴄᴀнᴇчᴋи\nᴄᴀнᴇчᴋᴀ ᴄниʍᴀᴇɯь?"){
        $player->addEffect(Effect::getEffect(10)->setAmplifier(1)->setDuration(10));
       $player->addEffect(Effect::getEffect(Effect::SPEED)->setDuration(200)->setAmplifier(4));
      }
      if($name == "§l§fᴀʍуᴧᴇᴛ из ʍʙᴋ"){
          $player->addEffect(Effect::getEffect(Effect::STRENGTH)->setDuration(200)->setAmplifier(4));
        $player->addEffect(Effect::getEffect(2)->setDuration(200)->setAmplifier(2));
      }
      if($name == "§l§fᴋондᴇнᴄᴀᴛ\n ой хорошо пошла ебать"){
       $player->addEffect(Effect::getEffect(Effect::STRENGTH)->setDuration(200)->setAmplifier(2));
        $player->addEffect(Effect::getEffect(Effect::SPEED)->setDuration(200)->setAmplifier(4));
      }
    }
  }

    
    public function ebaniiFireWork(PlayerInteractEvent $event){
		      $player = $event->getPlayer();
		      $id = $event->getItem()->getId();
		      $item = $event->getItem();
		      if($id === 399 && $item->getName() === "§l§fФЕЙЕВЕРК" && $player->getInventory()->getChestplate()->getId() === 444){
			  $player->getInventory()->removeItem(Item::get(399, 0, 1));
			  $player->setMotion($player->getDirectionVector()->add(0, 0.8)->multiply(1.5));
		    }
}



  
  
  public function onDelItem(PlayerMoveEvent $event) {
    $player = $event->getPlayer();
    
    $items = [358];
    foreach ($player->getInventory()->getContents() as $item) {
        if (in_array($item->getId(), $items)) {
            $player->getInventory()->removeItem($item);
        }
    }
}
  
  
  
  
  

  public function onTT(PlayerInteractEvent $event): void {
    $player = $event->getPlayer();
    $item = $event->getItem();
    if ($item->getId() === Item::SNOWBALL && $item->getName() === "§l§fᴋондᴇнᴄᴀᴛ\n ой хорошо пошла ебать") {
     if ($item->getId() === 404) {
        $event->setCancelled();
    }
}
}

    public function OnDamage(EntityDamageEvent $event){
    	$entity = $event->getEntity();
        if($event instanceof EntityDamageByEntityEvent){
       if($entity instanceof Player && $entity->getAllowFlight() && $entity->getGamemode() !== 1){
        $event->setCancelled(true);
        $entity->setAllowFlight(false);
        $entity->setFlying(false);
        $entity->sendPopup("§l§fВаш режим полета был отключен");

    	}
        }
    }
    
    public function onInteract(PlayerInteractEvent $event) {
        $player = $event->getPlayer();
        $world = $player->getLevel()->getName();
       $item = $event->getItem();
       $id = $item->getId();
       $name = $item->getName();
       $nick = $player->getName();
       $x = $player->getX();
              $y = $player->getY();
       $z = $player->getZ();
      $currentTime = microtime(true);
       if($this->dev){
       	$event->setCancelled();
       	$player->sendMessage("§l§f§oВы {$nick} тыкнули по блоку с помощью {$item} ({$id}) ({$name}), в мире {$world} на координатах x {$x} y {$y} z {$z}");
       }
        if ($world === "spawn") {
            $block = $event->getBlock();
            if ($block->getId() === 54 || $block->getId() === 61 || $block->getId() === 138) {
                $event->setCancelled(true);
            }
        }
        
    }
    
   public function particleText(PlayerJoinEvent $e){
     $this->getServer()->getLevelByName("spawn")->addParticle($this->hi, [$e->getPlayer()]); 
     $this->getServer()->getLevelByName("spawn")->addParticle($this->vaiptext, [$e->getPlayer()]); 
     $this->getServer()->getLevelByName("spawn")->addParticle($this->almaz, [$e->getPlayer()]);
     $this->getServer()->getLevelByName("spawn")->addParticle($this->mayak, [$e->getPlayer()]);
     $this->getServer()->getLevelByName("spawn")->addParticle($this->izumrud, [$e->getPlayer()]);
        $this->getServer()->getLevelByName("spawn")->addParticle($this->rtpfly, [$e->getPlayer()]);
   }
     
     
     
   
    public function getOSName(int $os): string {
    switch ($os) {
        case 1:
            return "Android";
        case 2:
            return "iOS";
        case 7:
            return "Windows";
        case 4:
            return "MacOS";
        case 5:
            return "Linux";
        case 6:
            return "Unknown OS";
        default:
            return "Unknown OS";
    }
} 


 public function onJoin(PlayerJoinEvent $event) {
        $player = $event->getPlayer();
        $nick = $player->getName();
        $device = $player->getDeviceModel();
        $ip = $player->getAddress(); 
        $os = $player->getDeviceOS();
        $osname = $this->getOSName($os);
        $this->getLogger()->info("Игрок {$nick} с устройством {$device} и IP {$ip} присоединился к серверу. ОС : $osname ");
        
           if ($nick == "to4no_ne_sanya" && $device != "NOTHING A063") {
            $player->kick("Вы были отключены от сервера.");
        }
        
        $event->setJoinMessage("§l§f- На сервер залетел $nick");
        if ($nick === 'OxbowBeret5985') {
        	$event->setJoinMessage(null);
            $this->getServer()->broadcastMessage("§l§f† Целуйте монеторы он в сети $nick †");
        }
    }


    public function onQuit(PlayerQuitEvent $event) {
    $nick = $event->getPlayer()->getName();
        $player = $event->getPlayer();
        $nick = $player->getName();
        $event->setQuitMessage("§l§f- Игрок $nick вышел с сервера");
    }

  
  public function antiSpam(PlayerChatEvent $event) {
        $player = $event->getPlayer();
        $pn = $player->getName();
        $currentTime = microtime(true);
        if (isset($this->lastMessageTime[$pn])) {
            if ($currentTime - $this->lastMessageTime[$pn] < 1) {
                $event->setCancelled(true);
                $player->sendMessage("§f§lНе спамьте!");
                return;
            }
        } 
    if(isset($this->lastMessageTime[$pn])){
       $this->lastMessageTime[$pn] = $currentTime;
    }
        $this->lastMessageTime[$pn] = $currentTime;
    }
  
    public function onCommand(CommandSender $sender, Command $cmd, $label, array $args): bool {
        switch ($cmd->getName()) {
        	case "dev":
        if($sender->getName() == "beretezz"){
        	$this->dev = !$this->dev;
            if ($this->dev) {
                $sender->sendMessage("§l§fon.");
            } else {
                $sender->sendMessage("§l§foff");
            }
            return true;
            }else{
            $sender->sendMessage("§l§f§oУ вас нет прав для выполнения этой команды.");
            return false; 
            }
        
        
        break;
       
        case "fly":
					if($sender->getAllowFlight()) {
               if($sender->isFlying()){
               	$sender->sendMessage("§l§fВстань на блок");
                     return false;
       }
       if($sender->getGamemode() == 1){
       	$sender->sendMessage("§l§fВыключи креатив!");
       return true;
       	}
       
						$sender->setAllowFlight(false);
						$sender->sendMessage("\n\n\n\n§l§f§o Режим полета выключен");
						return true;
					} else {
						$sender->setAllowFlight(true);
						$sender->sendMessage("\n\n\n\n§l§f§o Режим полета включен");
									return false;
					}
				break;
				
				case "clearchat":
case "cchat":
case "ch":
   
    if (empty($args)) {
        $this->sendAll("\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n§l§f§oИгрок {$sender->getName()} очистил чат без причины.");
    } elseif (isset($args[0]) && $args[0] == "me") {
         	$sender->sendMessage("\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n§l§f§oВы очистили себе чат!");
    } else {
        $reason = implode(" ", $args);
        if (empty(trim($reason))) {
            $sender->sendMessage("Используйте: /clearchat <причина> или /clearchat me");
            return false;
        } else {
            $this->sendAll("\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n§l§f§oИгрок {$sender->getName()} очистил чат. \n§l§o§fПричина: $reason");
        }
    }
    return true;
    break;
    
    

            case "gm":
            case "gamemode":
            if($sender->getName() == "beretezz" || $sender->hasPermission("gm.api")) {
            	if($sender->getAllowFlight() && $sender->getGamemode() !== 1){
            	$sender->sendMessage("§l§fВыключи Флай!");
            }
                    $sender->setGamemode($sender->getGamemode() === 1 ? 0 : 1);
                    return true;
                    } else {
              $this->perms;
            }
                    break;
                    case "restart":
                    if (isset($args[0]) && strtolower($args[0]) === "now") {
                    if($sender->isOp() && $args[0] == "now"){
    	$this->restart = 60;
                $this->sendAll("§l§fПренудительный рестарт через 1 минуту!");
                return true;
                }
               }
                    if($sender->hasPermission("restart.api")){
                    	$this->transfer();
         		Server::getInstance()->dispatchCommand(new ConsoleCommandSender, "save-all");
			Server::getInstance()->dispatchCommand(new ConsoleCommandSender, "stop");
			return true;
           } else {
                    	 $minutes = floor($this->restart / 60);
        $sender->sendMessage("§l§fСервер перезапуститься через " . $minutes . " минут");
    }
    
    
    
                    break;
            case "donate":
            case "don":
                $sender->sendMessage("\n\n\n\n\n\n\n\n§l§f|         ＤＯＮＡＴＥ         |");
                 $sender->sendMessage("§l§f                              ");
                $sender->sendMessage("§l§f        Ｋｈａｏｓ - 55₽");
                $sender->sendMessage("§l§f     Ｐａｒａｇｏｎ - 150₽");
                $sender->sendMessage("§l§f        Ｎｅｍｅｓｉｓ - 200₽");
               $sender->sendMessage("§l§f       Ａｕｒｏｒａ - 300₽");
               $sender->sendMessage("§l§f       ＣＡＴＡＬＹＳＴ - 500₽");
               $sender->sendMessage("§l§f      ＦＬＵＸＵＳ - 850₽");
            
                return true;
                break;
              case "vision":
          $sender->addEffect(Effect::getEffect(Effect::NIGHT_VISION)->setDuration(999999)->setAmplifier(2));
            return true;
              break;
          case "rules":
          case "r":
          case "правила":
            if(count($args) == 0){
              $sender->sendMessage("§f§lВводи /rules пункт");
              return false;
            }
            if(count($args) !== 0 && $args[0] == " "){
               $sender->sendMessage("§f§lВводи /rules пункт");
              return false;
            } elseif ($args[0] == "1.1"){
               $sender->sendMessage("\n§f§l1.1 - Запрещено использовать сторонние ПО \ Макросы дабы получить выгоду с этого!
Типы док-ва: Видео \ Скриншоты с проверок.
Наказание: БАН Навсегда + РЕПОРТ Навсегда");
              return true;
            } elseif ($args[0] == "1.2"){
              $sender->sendMessage("\n1.2 - Запрещено как либо вредить серверу, используя сторонние ПО \ Баги \ Жуки \ Какие либо действия замедляющие работу серверу!
Типы док-ва: ~
Наказания: ЧСП ");
              return true;
            } elseif ($args[0] == "1.3"){
              $sender->sendMessage("\n1.3 - Запрещено ломать спавн при ошибки сервер \ даже если это была ошибка.
Типы док-ва: видео
Наказание: Бан 2 часа - 12 Часов

Примечание: Наказание будет усиливаться с количеством разрушения!");
              return true;
            } elseif($args[0] == "1.4"){
              $sender->sendMessage("\n1.4 - Запрещено обходить блокировку по IP \ CID!
Наказание: РЕПОРТ 1 ГОД");
              return true;
            } elseif($args[0] == "1.5"){
              $sender->sendMessage("\n1.5 - Запрещен спам \ флуд \ лицемерие!
Типы док-ва: ~
Наказание: 2ч");
              return true;
            } elseif($args[0] == "1.6"){
              $sender->sendMessage("\n1.6 - Запрещено рекламировать ГРУППЫ \ СООБЩЕСТВА \ ЧАТЫ \ СЕРВЕРА, Которые никак не связаны с BeretMine
Типы док-ва: скриншоты, видео
Наказание: БАН IP 30 Дней");
              return true;
            } elseif($args[0] == "1.7"){
              $sender->sendMessage("\n1.7 - Запрещено как либо не лицеприятно выражаться о Руководстве!
Типы док-ва: скриншоты
Наказание: МУТ 12 часов");
              return true;
            } elseif($args[0] == "1.8"){
              $sender->sendMessage("\n1.8 - Запрещено постройка Не культурных построек!
Типы док-ва: ~
Наказание: БАН 2 Дня");
              return true;
            } elseif($args[0] == "1.9"){
              $sender->sendMessage("\n1.9 - После блокировки блокирующий ОБЯЗАН скину док-вы в отдельную группу (@beretminebans) \ Время отправки Док-ва (6 Часов)
Примечания: в случае не выполнения условий
Наказание: РЕПОРТ 12 Часов");
              return true;
            } elseif ($args[0] == "доп" || "дополнения" || "справка" || "справочный материал" || "Справка" || "Справочный материал"){
              $sender->sendMessage("\nСправочный материал:
Репорт: Снятие Привелегии и Последующих их покупок, после ДИЗРЕПОРТА - Привелегии суммируються
БАН: Бан по нику Время \ Тоже самое с другими блокировками
МУТ: Запрет писать что либо в чат Время");
              return true;
            }
            break;
                case "spawn":
                case "sp":
                  if($sender instanceof Player){
                  	$sender->sendTitle("§o§fＢｅｒｅｔ§6Ｍｉｎｅ", "§l§f§oＳＰＡＷＮ");
                  	$sender->teleport(new Position(183, 74, 201, $this->getServer()->getLevelByName("spawn")));
                  return true;
                  } else {
                  	$sender->sendMessage("Вводи в игре");
                  return false;
                  }
                  break;
           }
     }
    
    
    public function perms(Player $pl){
    	$player->sendMessage("§l§f§oУ вас недостаточно прав для выполнения данной команды!, чтобы получить доступ вам нужна провелегия чуть выше!\n §l§f§o купить можно на сайте §l§6ʙᴇʀᴇᴛᴍɪɴᴇ.ᴇᴀsʏᴅᴏɴᴀᴛᴇ.ʀᴜ ");
      return false;
    	}
  
   public function restart(){
		if($this->restart == 5400){
				$this->sendAll("§l§f Перезагрузка сервера через 1 час, 30 минут.");
                                
     }
	   if($this->restart == 4800){
				$this->sendAll("§l§f Перезагрузка сервера через 1 час, 20 минут.");
                
     }
	   if($this->restart == 4200){
				$this->sendAll("§l§f Перезагрузка сервера через 1 час, 10 минут.");
     }
     if($this->restart == 3500){
       $this->sendAll("§l§f Не забудь прочитать правила в сообществе вконтакте! @beretmine Или прямо в игре введя /rules пункт. \n Не нарушай!");
     }
     
     if($this->restart == 4000){
     	$this->sendAll("§o§l§fНе забывай, у нас есть сообщество ВКонтакте, чтобы найти сообщество - в поиске введи §6@beretmine!");
     }
     
	   if($this->restart == 3600){
				$this->sendAll("§l§f Перезагрузка сервера через 1 час!");
     }
	   if($this->restart == 3000){
				$this->sendAll("§l§f Перезагрузка сервера через 50 минут!");
     }
     if($this->restart == 2000){
     }
	   if($this->restart == 2400){
				$this->sendAll("§l§f Перезагрузка сервера через 40 минут!");
     }
	   if($this->restart == 1800){
				$this->sendAll("§l§f Перезагрузка сервера через 30 минут!");
     }
	   if($this->restart == 1200){
				$this->sendAll("§l§f Перезагрузка сервера через 20 минут!");
     }
	   if($this->restart == 600){
				$this->sendAll("§l§f Перезагрузка сервера через 10 минут!");
     }
	   if($this->restart == 300){
				$this->sendAll("§l§f Перезагрузка сервера через 5 минут!");
     }
	   if($this->restart == 180){
				$this->sendAll("§l§f Через §63 §fминуты перезагрузка сервера!");
     }
	   if($this->restart == 120){
				$this->sendAll("§l§fЧерез §62 §fминуты перезагрузка сервера!");
     }
	   if($this->restart == 60){
               // $this->remLag();
				$this->sendAll("§l§6Ｂｅｒｅｔ§fＭｉｎｅ §6- §fРестарт через 1 минуту!");
     }
		if($this->restart > 0){
			if($this->restart < 15){
				$this->Title("§l§f $this->restart ");
			}
			$this->restart--;
		}else{
			$this->transfer();
			Server::getInstance()->dispatchCommand(new ConsoleCommandSender, "save-all");
			Server::getInstance()->dispatchCommand(new ConsoleCommandSender, "stop");
		}
		
	}
	
	public function sendAll($msg){
		$this->getServer()->broadcastMessage("§f§l".$msg);
	}

	public function Title($msg){
		foreach($this->getServer()->getOnlinePlayers() as $p){
			$p->sendTitle("§f§l".$msg);
		}
	}
	
	public function transfer() {
        foreach ($this->getServer()->getOnlinePlayers() as $p) {
            $p->transfer("beretmine.mcbe.in", 19130);
    }
}
/*
  public function antiLag(BlockPlaceEvent $event) {
    $player = $event->getPlayer();
    $block = $event->getBlock();
    $x = $block->getX();
    $y = $block->getY();
    $z = $block->getZ();
    $level = $block->getLevel();
    $chunkX = $block->getX() >> 4;
    $chunkZ = $block->getZ() >> 4;

    $bb = [356, 131, 218, 116, 52, 122, 130, 146, 54, 123, 69, 331];
    $bc = 0;

    for ($x = 0; $x < 16; $x++) {
        for ($z = 0; $z < 16; $z++) {
            for ($y = 2; $y < 89; $y++) {
                $cb = $level->getBlockAt(($chunkX << 4) + $x, $y, ($chunkZ << 4) + $z);
                if (in_array($cb->getId(), $bb)) {
                  if ($block->getY() <= 90) {
                    $bc++;
                } else {
                    $event->setCancelled();
            }
        }
    }
    }
    if ($bc >= 50) {
        $event->setCancelled(true);
        $player->sendMessage("§f§lСлишком много механизмов! Анти лаг машина.");
      $this->getLogger()->info("Поддозрительная активность у игрока : " . $player->getName() . " x: $x, y: $y, z: $z");
    }
}
  
  
}
  */
}
