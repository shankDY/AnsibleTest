Vagrant.configure("2") do |config| # версия вагранта и дальше он нам экспозирует конфиг
	(1..5).each do |i| # создаем 5 машинок в цикле 
		config.vm.define "server#{i}" do |web| # далее определяем server
			web.vm.box = "ubuntu/focal64" # версия ОС, которую vagrant скачает со своих серверов и развернет
			web.vm.network "forwarded_port", id: "ssh", host: 2222 + i, guest: 22 # проброс портов
			web.vm.network "private_network", ip: "10.11.10.#{i}", virtualbox__intnet: true # создание сети. В нашем случае nat.
			web.vm.hostname = "server#{i}" #задаем hostname
            
			# копируем публичные ключи ssh. На наши сервачки.
			web.vm.provision "shell" do |s|
				ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
				s.inline = <<-SHELL
				echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys # для обычного пользака
				echo #{ssh_pub_key} >> /root/.ssh/authorized_keys # для рута
				SHELL
			end

			# конфигурируем нашу виртуалочку. 
			web.vm.provider "virtualbox" do |v| # выбираем гипервизор
				v.name = "server#{i}" # имя виртуальной машины
				v.memory = 2048 # выделяем оперативную память для виртуалочек
				v.cpus = 1 # количество vCPU для наших виртуалочек
			end
		end
	end
end
